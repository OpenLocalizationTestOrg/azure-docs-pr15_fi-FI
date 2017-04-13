<properties
   pageTitle="Ohjeet Windows Azure-Virtuaalikoneissa palvelimen Active Directoryn käyttöönotosta | Microsoft Azure"
   description="Jos tiedät AD-toimialueen palveluista ja AD-liittoutumispalvelut paikallinen ottamisesta, lue niiden vaikutus-Azuren näennäiskoneiden."
   services="active-directory"
   documentationCenter=""
   authors="femila"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/27/2016"
   ms.author="femila"/>

# <a name="guidelines-for-deploying-windows-server-active-directory-on-azure-virtual-machines"></a>Ohjeet käyttöönotto Windows Server Active Directory-Azure-virtuaalikoneissa

Tässä artikkelissa kerrotaan käyttöönottaminen Windows Server Active Directory ‑toimialueen palveluihin (AD DS) ja Active Directory Federation Services (AD FS) ja otat ne käyttöön Microsoft Azuren näennäiskoneiden paikallista välisiä eroja.

## <a name="scope-and-audience"></a>Laajuus ja käyttäjäryhmälle

Artikkeli on tarkoitettu käyttäjille, jotka on jo listan Active Directory paikalliseen käyttöönottoon. Se peittää Active Directoryn käyttöönotosta Microsoft Azuren näennäiskoneiden/Azure virtual verkkojen ja perinteinen paikallisten Active Directory-versioiden erot. Azure-virtuaalikoneissa ja Azure virtual verkkojen kuuluvat infrastruktuuri-muodossa--palvelun (IaaS) organisaatioissa ojentamassa hyödyntää tietojenkäsittely resurssien pilveen.

Käyttäjille, jotka eivät ole tottunut käyttämään Active Directory-käyttöönotto-kohdassa sopiva [AD DS: N käyttöönotto-oppaassa](https://technet.microsoft.com/library/cc753963) tai [AD FS-käyttöönoton suunnitteleminen](https://technet.microsoft.com/library/dn151324.aspx) .

Tässä artikkelissa oletetaan, että lukija on tuttuja seuraavia käsitteitä:

- Windows Server AD DS: N käyttöönottoa ja hallinta
- Käyttöönotto ja määrittäminen DNS tukemaan Windows Server AD DS-infrastruktuuri
- Windows Server AD FS käyttöönotto ja hallinta
- Käyttöönotosta, määrittämisestä ja hallinnasta varmenteen käyttäjän parannetulla (sivustot ja web services), voit käyttää Windows Server AD FS tunnusten
- Yleiset virtuaalikoneen käsitteistä, kuten määrittäminen virtual machine, virtual levyille ja virtual käyttäminen

Tässä artikkelissa esitellään hybrid käyttöönottotapa, mitkä Windows Server AD DS: ÄÄN tai AD FS ovat osittain käyttöön paikallisen ja ottaa osittain Azuren näennäiskoneiden vaatimukset. Asiakirjan kattaa ensin kriittiset erot käytössä Windows Server AD DS ja AD FS Azuren näennäiskoneiden ja paikallisen ja tärkeiden päätösten, jotka vaikuttavat suunnittelu ja käyttöönotto. Paperin muiden kerrotaan kunkin tarkemmin päätös asioista ohjeita ja ottamisesta käyttöön Profiilikuva eri käyttöönoton tilanteissa.

Tässä artikkelissa ei keskustella [Azure Active Directory](http://azure.microsoft.com/services/active-directory/)-määritys, joka on REST-pohjaisessa palvelussa, joka tarjoaa jäsenyyksien hallinta ja ohjausobjektin käyttömahdollisuudet cloud sovellukset. Azure Active Directory (Azure AD) ja Windows Server AD DS on kuitenkin suunniteltu toimimaan yhdessä tunnistetietojen ja käytön management-ratkaisun antamaan kuluvan päivän hybrid IT ympäristöissä ja nykyaikaisen sovellukset. Auttaa ymmärtämään erot ja Windows Server AD DS ja Azure AD välisiä suhteita, toimi seuraavasti:

1. Ehkä suoritat Windows Server AD DS pilveen Azuren näennäiskoneiden käytettäessä Azure voit laajentaa paikallisen palvelinkeskuksen pilveen.
2. Voit käyttää Azure AD voi antaa oman käyttäjien kertakirjautumisen ohjelmiston nimellä--palvelun (SaaS)-sovelluksia. Microsoft Office 365 käyttää tätä tekniikkaa esimerkiksi ja Azure tai cloud muiden ympäristöjen sovellusten myös käyttää sitä.
3. Voit käyttää Azure AD (sen Access Control-palvelu) käyttäjät voivat kirjautua sisään käyttämällä käyttäjätietojen Facebook, Google, Microsoftin ja muiden tunnistetietojen toimittajat-sovellukset, joita isännöidään cloud tai paikallisen.

Lisätietoja nämä erot on artikkelissa [Azure tunnistetiedot](fundamentals-identity.md).

## <a name="related-resources"></a>Aiheeseen liittyvät resurssit

Voit ladata ja [Azure virtuaalikoneen valmiuden](https://www.microsoft.com/download/details.aspx?id=40898)suorittaa. Arvioinnin automaattisesti Tarkasta paikallisen ympäristön ja luo mukautettu raportti tämän ohjeen avulla voit siirtää ympäristön Azure-kohdan ohjeiden mukaisesti.

On suositeltavaa, että luet myös ensin opetusohjelmat, apuviivat ja videoita, joissa käsitellään seuraavia aiheita:

- [Määrittää vain Pilvipalveluita Virtual verkon Azure-portaalissa](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)
- [Määritä sivusto sivusto VPN Azure-portaalissa](../vpn-gateway/vpn-gateway-site-to-site-create.md)
- [Asenna uusi Active Directory-metsää Azure virtual verkossa](active-directory-new-forest-virtual-machine.md)
- [Azure replikan Active Directory-toimialueen ohjauskoneen asentaminen](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)
- [Microsoft Azure IT Pro IaaS: (01) virtuaalikoneen perusteet](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
- [Microsoft Azure IT Pro IaaS: (05) luominen Virtual verkkojen ja paikallisen-yhteys](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)

## <a name="introduction"></a>Johdanto

Käyttöönotto Windows Server Active Directory-Azuren näennäiskoneiden ratkaisevan vaatimukset eroavat hyvin vähän-sen käyttöönottamisesta paikallisen näennäiskoneiden (ja joissakin tapauksissa fyysisten laitteiden). Esimerkiksi kyseessä Windows Server AD DS, jos toimialueen ohjauskoneet, voit ottaa käyttöön Azuren näennäiskoneiden on replikoiden aiemmin luodun paikalliset yrityksen toimialueen ja metsää, sitten Azure käyttöönoton suurelta voidaan käsitellä samalla tavalla kuin minkä tahansa muita Windows Server Active Directory-sivuston voi käsitellä. Toisin sanoen aliverkosta on määritettävä Windows Server AD DS, luotu sivusto, aliverkosta sivuston linkitetty, ja yhdistää muihin sivustoihin käyttämällä sivuston-linkeistä. On-kuitenkin joitakin eroja, jotka ovat yhteiset kaikki Azure ominaisuuksissa ja jotkin, vaihdella sen mukaan, tietyn käyttöönottotapa. Seuraavassa esitetyt kaksi keskeisiä eroja:

### <a name="azure-virtual-machines-may-need-connectivity-to-the-on-premises-corporate-network"></a>Azuren näennäiskoneiden on yhteys paikalliseen yrityksen verkkoon.

Yhteyden Azuren näennäiskoneiden takaisin paikalliseen yritysverkon edellyttää Azure virtual verkkoon, joka sisältää sivuston sivuston tai sivuston piste virtual yksityinen verkko (VPN)-osan voi muodostaa saumattomasti Azuren näennäiskoneiden ja paikallisen koneet. VPN-komponenttia myös hyödyntämällä paikallisen toimialueen jäsentietokoneet käyttämään Windows Server Active Directory-toimialueen, jonka toimialueen ohjaimet isännöidään yksinomaan Azuren näennäiskoneiden. On tärkeää muistaa, että VPN-yhteys epäonnistuu, jos todennus ja muita toimintoja, jotka riippuvat Windows Server Active Directory epäonnistuu. Kun käyttäjät saattavat voi enää Kirjaudu sisään aiemmin välimuistiin tallennetut tunnistetiedot, kaikki-vertaisääni tai asiakkaan ja palvelimen todennuksen yritykset, jonka liput on vielä voimassa tai on tullut vanhentuneita epäonnistuu.

Katso video osoitus ja opetusohjelmia, mukaan lukien [Määritä sivusto sivusto VPN Azure-portaalissa](../vpn-gateway/vpn-gateway-site-to-site-create.md)luettelo [VPN](http://azure.microsoft.com/documentation/services/virtual-network/) .

> [AZURE.NOTE] Voit myös ottaa Windows Server Active Directory Azure virtual verkossa, joka ei ole yhteys paikalliseen verkkoon. Tämän artikkelin ohjeiden oletetaan, että Azure virtual verkon käytetään, koska se sisältää IP-osoitteiden ominaisuuksista, joita tarvitaan Windows Server.

### <a name="static-ip-addresses-must-be-configured-with-azure-powershell"></a>Staattinen IP-osoitteiden on oltava määritettynä Azure PowerShellin avulla.

Dynaaminen osoitteet varataan oletusarvon mukaan, mutta määrittäminen AzureStaticVNetIP cmdlet-komennon avulla voit määrittää sen sijaan staattinen IP-osoite. Asettaa staattinen IP-osoite, joka on käytössä palvelun kenties ja AM Sammuta tai käynnistä se uudelleen. Lisätietoja on artikkelissa [staattinen näennäiskoneiden sisäinen IP-osoite](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/).

## <a name="BKMK_Glossary"></a>Termit ja määritykset

Seuraavassa on eri Azure tekniikoita, johon viittaa tämän artikkelin ehdot muun muassa luettelo.

- **Azure-virtuaalikoneissa**: IaaS tarjoaa Azure-tietokannassa, jonka avulla asiakkaat voivat ottaa käyttöön virtuaalilaitteiksi lähes minkä tahansa perinteisesti paikallisen palvelimen työmäärää.

- **Azure virtual verkon**: Azure-tietokannassa, jonka kautta asiakkaat, luoda ja hallita Azure virtual verkot suojatusti linkittää niitä omia verkostopalveluiden paikallisen verkkoinfrastruktuuria näennäisen yksityisverkon (VPN) avulla.

- **Virtuaalinen IP-osoite**: Internetiin yhteydessä oleva IP-osoitetta, joka ei ole sidottu tietokoneesta tai verkosta käyttöliittymän kyseisen kortin. Cloud Services-palvelut on määritetty vastaanottamiseen verkkoliikennettä, jossa ohjataan Azure-AM virtual IP-osoite. Näennäinen IP-osoite on ominaisuus, jotka voivat sisältää vähintään yhden Azuren näennäiskoneiden cloud-palvelun. Huomaa, että Azure virtual verkon voi sisältää vähintään yhden pilvipalveluihin. Virtuaalinen IP-osoitteiden on alkuperäisen kuormituksen tasaamisen ominaisuuksia.

- **Dynaaminen IP-osoite**: Tämä on on sisäinen IP-osoitetta. Se on määritetty kuin staattinen IP-osoite (käyttämällä määrittäminen AzureStaticVNetIP cmdlet-komento), jotka isännöivät Ohjauskoneen/DNS server rooleja VMs.

- **Palvelun kenties**: prosessi, jossa Azure automaattisesti palauttaa palvelu käytössä olevaan tilaan uudelleen sen jälkeen, kun se havaitsee, että palvelu on epäonnistunut. Palvelun kenties on Azure, joka tukee käytettävyys ja vikasietoisuudelle ominaisuuksia. Samalla, kun se on epätodennäköistä, seuraa palvelun tapahtumasta kenties varten käytössä AM Ohjauskoneen tulos muistuttaa suunnittelematon uudelleenkäynnistys, mutta on muutama vaikutusten:

 - VPN-sovittimen AM muuttuu
 - Virtuaalinen verkkosovittimen MAC-osoite muuttuu
 - AM suoritin/suorittimen tunnus muuttuu
 - Virtuaalinen verkkosovittimen IP-määritys ei muutu, kunhan AM liitetään virtual verkossa ja AM IP-osoite on staattinen.

 Ei mitään toiminta vaikuttaa Windows Server Active Directory, koska siinä ei ole riippuvuuden MAC-osoite tai suoritin/suorittimen tunnuksen ja kaikki Windows Server Active Directory-käyttöönotoissa Azure-suositellaan voidaan suorittaa Azure virtual verkon edellä kuvatulla.

## <a name="is-it-safe-to-virtualize-windows-server-active-directory-domain-controllers"></a>Onko turvallista virtualisoi Windows Server Active Directory-toimialueen ohjaimet?

Windows Server Active Directory ohjauskoneita käyttöönotto-Azuren näennäiskoneiden peritään samoja ohjeita kuin suorittaminen ohjauskoneita paikallisen virtual machine. Käynnissä virtualisoidussa ohjauskoneita on turvallinen käytäntö, kunhan varmuuskopiointia ja palauttamista ohjauskoneita ohjeita noudatetaan. Saat lisätietoja rajoitteiden ja ohjeet käynnissä virtualisoidussa ohjauskoneita [Käynnissä toimialueen ohjaimet Hyper-V](https://technet.microsoft.com/library/dd363553).

Hypervisors antaa tai trivialize toiminnot, jotka saattavat aiheuttaa ongelmia useita eri aikajaksoille Systemsin, mukaan lukien Windows Server Active Directory. Esimerkiksi fyysinen palvelimessa, voit Kloonaa DVD-levyllä tai joita ei tueta menetelmillä palauttamaan palvelimen, sekä käyttämään SANs ja niin edelleen tila, mutta tämä fyysinen palvelimessa on hankala kuin palauttaminen virtuaalikoneen-tilannevedoksen hypervisor. Azure on toimintoja, mikä voi johtaa sama ei-toivottujen ehto. Esimerkiksi olisi ei kopioida sitten välin näennäiskiintolevytiedostoja sijaan suorittamiseen säännöllisen varmuuskopioinnin suunnittelu, koska palauttamisesta voi johtaa samalla tilanteessa tilannevedoksen palauttaminen ominaisuuksilla.

Näiden palautukset esitellä USN Kuplat, joka aiheuttaa pysyvästi eriäviä hyötyä ohjauskoneita välillä. Jotka saattavat aiheuttaa ongelmia seuraavasti:

- Jäävät objektit
- Taulukossa on ristiriitainen salasanat
- Taulukossa on ristiriitainen määritteiden arvot
- Jos rakenteen perustyyli peruutetaan rakenteen lähdeluetteloiden

Saat lisätietoja siitä, miten ohjauskoneet ovat vaikuttaneet [USN ja USN Peru](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx#usn_and_usn_rollback).

Alkavat Windows Server 2012, [Lisää valvontaa on suunniteltu AD DS: ÄÄN](https://technet.microsoft.com/library/hh831734.aspx). Nämä valvontaa auttaa suojaamaan virtualisoidussa toimialueen ohjaimet vastaan edellä mainitut ongelmat, kunhan pohjana hypervisor-ympäristö tukee AM GenerationID. Azure tukee AM GenerationID, mikä tarkoittaa, että toimialueen ohjaimet, joissa on Windows Server 2012: ssa tai uudempi Azuren näennäiskoneiden on muita valvontaa.

> [AZURE.NOTE] Olisi Sammuta ja Käynnistä AM, joka suoritetaan toimialueen ohjauskoneen rooli Azure Vieras-käyttöjärjestelmässä Azure perinteinen portaalissa **Sammuta** -vaihtoehdon sijaan. Nykyään AM Sammuta perinteinen portaalin avulla aiheuttaa AM, varaus on poistettu. Deallocated AM ei lisäkustannuksia etuna on, mutta se myös palauttaa AM GenerationID, joka on toimialueen Ohjauskoneen haitalliset. Kun AM-GenerationID on palautettu, AD DS-tietokannan invocationID myös Palauta, RID-varanto hylätään ja SYSVOL on merkitty ei-tärkeitten. Lisätietoja on artikkelissa [Johdanto Active Directory-toimialueen palveluista (AD DS) virtualisointi](https://technet.microsoft.com/library/hh831734.aspx) ja [Määritetyissä DFSR turvallisesti](http://blogs.technet.com/b/filecab/archive/2013/04/05/safely-virtualizing-dfsr.aspx).

## <a name="why-deploy-windows-server-ad-ds-on-azure-virtual-machines"></a>Miksi ottaa Windows Server AD DS-Azuren näennäiskoneiden?

Windows Server AD DS: N käyttöönottoskenaarioita ovat sovellu käyttöönoton VMs Azure-muodossa. Oletetaan, että sinulla yrityksen Euroopan todentaa käyttäjät etäkohteeseen Aasiassa korjattava. Yrityksen ei aiemmin otettu käyttöön Windows Server Active Directory ohjauskoneita Aasiassa vuoksi ottamaan ne ja rajoitettu osaamisalueet hallittavan palvelinten käyttöönoton jälkitoimet kustannukset. Tämän vuoksi Aasian todennus-pyynnöt ovat palvelee ohjauskoneita Euroopan lainkaan tuloksia. Tässä tapauksessa Ohjauskoneen AM, jonka olet määrittänyt on suoritettava Aasiassa Azure palvelinkeskukseen, voit ottaa käyttöön. Liittäminen kyseisen toimialueen Ohjauskoneen virtual Azure-verkkoon, joka on suoraan yhteydessä etäkohteeseen parantaa todennus suorituskykyä.

Azure sopii myös hyvin hyödyntää korvaavana muussa kallista tietojen palauttaminen (DR) sivustoihin. Suhteellisen vähän-kustannusten isännöivän on pieni määrä toimialueohjaimia ja yksittäisen virtual verkossa Azure edustaa houkuttelevien löytämiseen.

Lopuksi haluat ottaa Azure, kuten SharePoint-, joka edellyttää Windows Server Active Directory, mutta ei ole riippuvuuden paikalliseen verkkoon tai yrityksen Windows Server Active Directory-verkkosovelluksen. Tässä tapauksessa käyttöönotto erillään metsää, valitse Azure täyttävän SharePoint Serverin vaatimukset on paras mahdollinen. Uudelleen käyttöönotto verkkosovellukset edellyttävät yhteys paikalliseen verkkoon ja yrityksen Active Directorysta tuetaan myös.

> [AZURE.NOTE] Koska se sisältää layer 3-yhteyden, VPN-osa, joka tarjoaa Azure virtual verkko- ja paikallisen verkon väliset yhteydet voit myös ottaa jäsenpalvelimissa, jotka suoritetaan paikalliseen hyödyntää ohjauskoneita, joka suoritetaan Azuren näennäiskoneiden Azure virtual verkossa. Mutta VPN-yhteys ei ole käytettävissä, jos välisen paikalliseen tietokoneeseen ja toimialueen Azure-ei toimi, tuloksena olevat todennus ja eri virheitä.  

## <a name="contrasts-between-deploying-windows-server-active-directory-domain-controllers-on-azure-virtual-machines-versus-on-premises"></a>Eroaa välillä Windows Server Active Directory toimialueen ohjaimet-Azuren näennäiskoneiden paikallisen ja käyttöönotto

- Mikä tahansa Windows Server Active Directory käyttöönoton tilanteessa, jossa on enemmän kuin yksi AM on tarpeen Azure virtual verkon käytettävä IP-osoite yhdenmukaisuuden. Huomaa, että tässä oppaassa oletetaan, että ohjauskoneet ovat käytössä Azure virtual verkko.

- Paikallisen ohjauskoneita, jossa on suositeltavaa staattinen IP-osoitteet. Staattinen IP-osoite voidaan määrittää vain Azure PowerShell-toiminnolla. Saat lisätietoja [staattinen VMs sisäinen IP-osoite](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/) . Jos sinulla on seuranta järjestelmät tai muita ratkaisuja, tarkista staattinen IP-osoite määritys Vieras-käyttöjärjestelmässä, voit määrittää staattinen IP-osoite AM verkkosovittimen ominaisuuksia. Mutta huomioon, että verkkosovittimen hylätään, jos AM käy läpi palvelun kenties tai perinteinen portaalissa suljetaan ja on osoite Varaus poistettu. Tässä tapauksessa sisällä Vierailijan staattinen IP-osoite on palautetaan.

- Käyttöönottoon VMs virtual verkon ei ilmentävät (tai vaadi) connectivity takaisin paikalliseen-verkon; virtuaalinen verkon mahdollistaa pelkästään tämä mahdollisuus. Sinun on luotava virtual verkon yksityinen Azure ja paikallisen verkon väliseen viestintään. Haluat ottaa VPN-päätepisteen paikallisen verkossa. Azure avatut VPN-yhteys paikalliseen verkkoon. Lisätietoja on artikkelissa [Virtual verkko-yleiskatsaus](../virtual-network/virtual-networks-overview.md) ja [Määritä sivusto sivusto VPN Azure-portaalissa](../vpn-gateway/vpn-gateway-site-to-site-create.md).

> [AZURE.NOTE] Mahdollisuus [luoda pisteen sivuston VPN-yhteyttä](../vpn-gateway/vpn-gateway-point-to-site-create.md) ei käytettävissä yhteyden yksittäisiä Windows-tietokoneissa Azure virtual verkossa.

- Riippumatta siitä, onko näennäiskansio verkon tai juniin liikenne mutta ei tunkeutumisen Azure kulut. Eri Windows Server Active Directory-rakenne-vaihtoehtojen voivat vaikuttaa siihen, kuinka paljon juniin liikenne luodaan käyttöönoton. Esimerkiksi käyttöönotto vain luku-toimialueen ohjauskoneen (Ohjauskoneen) rajaa juniin liikenne koska ei replikoida lähtevä. Mutta päätös käyttöönotto Ohjauskoneen on arvioitava suhteessa ei tarvitse suorittaa kirjoittaminen Palvelinkeskus ja [yhteensopivuuden](https://technet.microsoft.com/library/cc755190) , sovellusten ja palveluiden sivuston työskentelee RODCs vastaan. Lisätietoja liikenne ovat artikkelissa [Azure hinnat osoitteessa--nopeasti](http://azure.microsoft.com/pricing/).

- Kun sinulla on valmis hallita kautta mitä palvelimen resursseja VMs paikallisen, kuten RAM-Muistia levyn kokoa, ja Azure-niin edelleen, sinun on valittava esimääritettyjä palvelimen koot luettelosta. Toimialueen Ohjauskoneen tietojen DVD-levyllä on tarvittavat käyttöjärjestelmän levyn lisäksi haluat tallentaa Windows Server Active Directory-tietokantaan.

## <a name="can-you-deploy-windows-server-ad-fs-on-azure-virtual-machines"></a>Voit ottaa Windows Server AD FS-Azuren näennäiskoneiden?

Kyllä, voit ottaa käyttöön Windows Server AD FS-Azuren näennäiskoneiden ja [AD FS käyttöönoton parhaat käytännöt](https://technet.microsoft.com/library/dn151324.aspx) paikallisen koskevat tasaisesti Azure AD FS käyttöönoton. Mutta joitakin parhaita käytäntöjä, kuten kuormituksen tasaaminen ja suuren käytettävyyden edellyttävät lisäksi mitä AD FS tarjoaa itse tekniikoita. Ne on annettava pohjana oleva infrastruktuuri. Oletetaan, että Tarkista joitakin näiden parhaiden käytäntöjen ja katso, miten ne voidaan saavuttaa Azure VMs ja Azure virtual verkon avulla.

1. **Näytä aina suojauksen suojaustunnuksen service (STS)-palvelimiin yhteys Internetiin.**

    Tämä on tärkeää, koska STS ongelmat suojauksen tunnusten. Tämän vuoksi STS-palvelimet, esimerkiksi AD FS-palvelimet käsitellään kanssa samalla tasolla kuin toimialueen ohjauskoneen suojauksen. Jos STS-ympäristö on ongelmia, haitallisen käyttäjät voivat antaa access tunnusten sisältävät mahdollisesti haluamaansa varmenteen käyttäjän osapuolien sovellukset ja muissa STS palvelimissa luottaminen organisaatiot claims.

2. **Ota käyttöön Active Directory-toimialueen ohjaimet kaikki käyttäjän toimialueiden AD FS-palvelimet samassa verkossa.**

    AD FS-palvelimet todentaa käyttäjät Active Directory-toimialueen palveluiden avulla. On suositeltavaa ottaa käyttöön toimialueen ohjaimet samassa verkossa kuin AD FS-palvelimiin. Liiketoiminnan jatkuvuus on niin Azure verkko- ja paikallisen verkon välinen linkki katkeaa ja mahdollistaa alemman viive ja kirjautumiset parempi suorituskyky.

3. **Ottaa käyttöön useita AD FS-solmut hyvän käytettävyyden ja kuormituksen tasaamisen.**

    Useimmissa tapauksissa epäonnistumisen sovellus, joka mahdollistaa AD FS on voi hyväksyä, koska sovellukset, jotka edellyttävät suojauksen tunnusten ovat usein toiminta-ajatuksen kriittinen. Tämän vuoksi ja AD FS sijaitsee nyt kriittisen polun kriittinen tehtävä-sovellusten käyttäminen, koska AD FS-palvelun täytyy olla erittäin saatavana useita AD FS-välityspalvelimet ja AD FS-palvelimiin. Tavoitteet pyynnöt jakautumisen kuormituksen tasoitusmääritykset yleensä käyttöön eteen AD FS-välityspalvelimet ja AD FS-palvelimiin.

4. **Ota käyttöön vähintään yksi Web-sovelluksen välityspalvelimen solmujen internet-yhteys.**

    Kun käyttäjät on suojattu AD FS-palvelu-sovelluksia, AD FS-palvelu on oltava käytettävissä Internetistä. Tämä on mahdollista Web-sovelluksen välityspalvelu käyttöönotto. On erittäin suositeltavaa ottaa käyttöön useita solmu suuren käytettävyyden tarkoitetaan ja kuormituksen tasaus.

5. **Rajoita käyttöoikeuksia-Web-sovelluksen välityspalvelimen solmut sisäisen verkkoresursseja.**

    Ulkoiset käyttäjät voivat käyttää AD FS Internetistä, sinun täytyy ottaa käyttöön Web-sovelluksen välityspalvelimen solmujen (tai AD FS ‑välityspalvelin Windows Server aiemmissa versioissa). Web-sovelluksen välityspalvelimen solmut suoraan niitä julkaista Internetissä. Niitä ei tarvitse olla toimialueen liittyneet ja tarvitsemansa AD FS-palvelimet vain TCP-portit 443 ja 80 päälle. On suositeltavaa, että kaikkien muiden tietokoneiden (erityisesti toimialueen ohjaimet) tietoliikenne on estetty.

    Tämä on yleensä saavuttaa paikallisen DMZ avulla. Palomuurit whitelist moodilla-toiminnon avulla voit rajoittaa DMZ-liikenne paikalliseen verkkoon (eli vain liikenne määritetty IP-osoitteista ja määritetyt portit sallitaan ja kaikki muut liikenne on estetty).

Kaavio on perinteinen seuraavassa paikalliset AD FS käyttöönotto.

![Perinteinen paikallisen Active Directory Federation Services-ympäristön kaavio](media/active-directory-deploying-ws-ad-guidelines/ADFS_onprem.png)

Kuitenkin Azure ei tarjoa alkuperäisen, täydet toiminnot sisältävien ohjelmien palomuurin ominaisuuksien muita vaihtoehtoja on voidaan rajoittaa liikenne. Seuraavassa taulukossa on kunkin asetuksen ja sen hyötyjä ja haittoja.

| Vaihtoehto | Käytä | Haitat |
| ------ | --------- | ------------ |
| [Azure verkon käyttöoikeusluettelot](virtual-networks-acl.md) | Vähemmän kallista ja yksinkertaisempi alkuperäinen määritys | Muita verkon Käyttöoikeusluettelon määritysten pakollinen Jos mikä tahansa uusi VMs lisätään käyttöönotto |
| [Barracuda maa-palomuurin](https://www.barracuda.com/products/ngfirewall) | Whitelist tila toiminnon ja se vaatii ei ole verkon Käyttöoikeusluettelon määrittäminen | Parannettu kustannusten ja asennuksen |

Ottaa käyttöön AD FS korkean tason vaiheet ovat tällöin seuraavasti:

1. Luo virtuaalisia verkon paikallisen-yhteyden, VPN-tai [ExpressRoute](http://azure.microsoft.com/services/expressroute/).

2. Ottaa käyttöön toimialueen ohjaimet virtual verkossa. Tämä vaihe on valinnainen, mutta suositeltava.

3. Ota käyttöön virtual verkon AD FS palvelimia toimialueeseen liittymistä.

4. Luo [sisäinen kuormituksen saapuva määrittäminen](http://azure.microsoft.com/blog/internal-load-balancing/) , joka sisältää AD FS-palvelimia ja uuden yksityisen IP-osoitteen virtual verkoston (dynaaminen IP-osoite).

  1. Päivitä DNS-Luo yksityinen (dynaamisen) IP-osoite, sisäinen kuormitus tasataan määrittäminen osoittamaan täydellinen toimialuenimi.

5. Luo Web-sovelluksen välityspalvelimen solmujen pilvipalveluun (tai erillisen virtual verkon).

6. Web-sovelluksen välityspalvelimen solmujen pilvipalveluun tai VPN käyttöön

  1. Luo ulkoinen kuormitus tasataan-joukko, joka sisältää Web-sovelluksen välityspalvelimen solmut.

  2. Päivitä ulkoinen DNS-nimi (FQDN) osoittamaan cloud palvelun julkiseen IP-osoite (virtual IP-osoite).

  3. Määritä AD FS-välityspalvelimet käyttämään FQDN, joka vastaa sisäinen kuormitus tasataan AD FS-palvelimia.

  4. Päivitä ulkoinen FQDN käytettävät saatavat niiden palveluntarjoajan väitepohjaista sivustoja.

7. Rajoita käyttöoikeuksia Web sovelluksen välityspalvelin koneen AD FS virtual verkon välillä.

Voit rajoittaa liikenne kuormituksen joukosta Azure sisäinen kuormituksen varten on määritettävä vain tietoliikenteen TCP-portit 80 ja 443 ja kaikki muu tietoliikenne kuormitus tasataan Määritä sisäisen dynaaminen IP-osoite, poistetaan.

![ADFS: N verkon käyttöoikeusluettelot TCP 443 + 80-kaavio on sallittua.](media/active-directory-deploying-ws-ad-guidelines/ADFS_ACLs.png)

AD FS-palvelimien liikenteen sallittava vain myös seuraavista lähteistä:

- Azure sisäinen kuormituksen.
- Järjestelmänvalvojan paikallisen verkon IP-osoite.

> [AZURE.WARNING] Rakenteen estettävä Web-sovelluksen välityspalvelimen solmujen ottamasta muiden VMs Azure virtual verkossa tai paikallisen verkon sijainteja. Joka voidaan toteuttaa määrittämällä palomuurisäännöt paikallisen laitteen Express reitti-yhteyksien tai sivuston sivuston VPN-yhteydet VPN-laitteessa.

Haitta tämä asetus on useita laitteita, esimerkiksi sisäinen kuormituksen AD FS-palvelimia ja muissa palvelimissa, jotka lisätään virtual verkon verkosta käyttöoikeusluettelot määrittämiseen tarvittavat. Jos laitteeseen on lisätty käyttöönoton määrittämättä verkon käyttöoikeusluettelot liikenne rajoittamaan, koko käyttöönotto voi olla vastuullasi. Jos Web-sovelluksen välityspalvelimen solmujen IP-osoitteiden koskaan muuttaa verkon käyttöoikeusluettelot on palautettava (mikä tarkoittaa, välityspalvelimet on määritettävä käyttämään [staattista dynaamisia IP-osoitteita](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/)).

![ADFS-Azure Käyttöoikeusluettelot verkoston kanssa.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure.png)

Toinen vaihtoehto on [Barracuda maa palomuuri](https://www.barracuda.com/products/ngfirewall) -laitteen avulla voit hallita liikenne AD FS-välityspalvelimet ja AD FS-palvelinten välillä. Tämä vaihtoehto noudattaa suojaus ja suuren käytettävyyden parhaat käytännöt ja edellyttää vähintään hallinnan ensimmäisen määrityskerran jälkeen, koska Barracuda maa palomuurin laitteen sisältää palomuurin hallinnan whitelist tilan ja se voidaan asentaa suoraan Azure virtual verkossa. Joka ei tarvitse määrittää verkon käyttöoikeusluettelot tahansa uuden palvelimen lisätään käyttöönotto. Mutta tämä vaihtoehto lisää alkuperäisen käyttöönoton monimutkaisuuden ja kustannukset.

Tässä tapauksessa kaksi virtual verkot on otettu käyttöön yhden sijaan. Olemme soittaessa heille VNet1 ja VNet2. VNet1 sisältää välityspalvelimet ja VNet2 sisältää STSs ja verkkoyhteys takaisin yrityksen verkkoon. VNet1 siis fyysisesti (tosin lähes) eristetty VNet2- ja puolestaan yrityksen verkosta. VNet1 yhdistetty VNet2 määräten tunnelointia tunnetut kuin Transport riippumaton verkoston arkkitehtuuri (TINA) avulla. TINA tunnelin on liitetty kunkin Barracuda maa palomuurilla virtual verkostojen – yksi Barracuda kunkin virtual verkot.  Suuren käytettävyyden on suositeltavaa ottaa käyttöön jokaisen virtual verkossa; kaksi barrakudoja. yhden aktiivisen muiden passiivinen. Ne tarjoavat erittäin rich firewalling ominaisuuksia, jotka mahdollistavat us jäljitteleminen perinteinen paikallisen-DMZ Azure-toimintoa.

![ADFS-Azure palomuuri.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure_firewall.png)

Lisätietoja on artikkelissa [AD FS: Laajenna saatavat tukeva paikallisen edusta sovellus Internet](#BKMK_CloudOnlyFed).

### <a name="an-alternative-to-ad-fs-deployment-if-the-goal-is-office-365-sso-alone"></a>Vaihtoehtoinen AD FS käyttöönoton Jos lähinnä Office 365: n SSO yksin

On toinen vaihtoehto käyttöönotossa AD FS kokonaan, jos lähinnä vain, jos haluat ottaa käyttöön kirjautumisnimi Office 365: ssä. Siinä tapauksessa voit yksinkertaisesti käyttöönotto DirSync salasanan synkronointi paikalliseen ja saavuttaa saman lopputuloksen mahdollisimman vähän käyttöönoton monimutkaisuus, koska tämä menetelmä ei edellytä AD FS tai Azure.

Seuraavassa taulukossa on Vertailtu toiminta-kirjautuminen prosessit kanssa ja ilman AD FS käyttöönotto.

| Office 365: n yksittäisen Sign AD FS ja Dirsync-työkalun avulla | Office 365 sama Sign käyttämällä DirSync + salasanan synkronointi |
| ------------- | ------------- |
| 1. käyttäjä kirjautuu sisään yrityksen verkkoon ja Windows Server Active Directory todennetaan. | 1. käyttäjä kirjautuu sisään yrityksen verkkoon ja Windows Server Active Directory todennetaan. |
| 2. käyttäjä yrittää käyttää Office 365: ssä (olen @contoso.com). | 2. käyttäjä yrittää käyttää Office 365: ssä (olen @contoso.com). |
| 3. office 365: n ohjaa käyttäjän Azure AD. | 3. office 365: n ohjaa käyttäjän Azure AD. |
| 4. koska Azure AD käyttäjä ei voi todentaa ja ymmärtää on AD FS paikallisen luottamus, se ohjaa käyttäjän AD FS. | 4. azure AD ei hyväksy Kerberos-lippujen suoraan ja luottamussuhde ei ole olemassa niin, että se pyytää käyttäjän kirjoitettava tunnistetiedot. |
| 5. käyttäjä lähettää Kerberos-lippu AD FS STS. | 5. käyttäjän kirjoittamat saman paikallisen salasanan ja Azure AD tarkistaa ne vastaan käyttäjänimi ja salasana, jotka on synkronoitu Dirsync-työkalun avulla. |
| 6. AD FS muunnokset tarvittavat suojaustunnuksen muoto ja saatavat Kerberos lippu ja ohjaa käyttäjän Azure AD. | 6. azure AD ohjaa käyttäjän Office 365: ssä. |
| 7. käyttäjän suorittaa Azure AD (toinen muunnos tapahtuu). |  7. käyttäjän voit kirjautua Office 365- ja OWA käyttämällä Azure AD-tunnuksen. |
| 8. azure AD ohjaa käyttäjän Office 365: ssä. |  |
| 9. käyttäjä on kirjautunut äänettömästi Office 365: een. |  |

Office 365 dirsyncillä ja salasanan synkronointi skenaario (ei AD FS) kertakirjautuminen korvataan "saman sign-on" Jos "sama" yksinkertaisesti tarkoittaa, että käyttäjät uudelleen voi syöttää tunnistetietoja saman paikallisen käytettäessä Office 365: ssä. Huomaa, että käyttäjän selaimen pakettitiedoston myöhemmin kehotteita kunnioitetaan mitalilla tiedoista.

### <a name="additional-food-for-thought"></a>Muita ruoka for haluat näyttää

- Jos otat käyttöön AD FS-välityspalvelimen, valitse Azure virtuaalikoneen, tarvitaan connectivity AD FS-palvelimiin. Jos ne ovat paikallisen, on suositeltavaa, että voit hyödyntää Web-sovelluksen välityspalvelimen solmuissa niiden AD FS-palvelinten kanssa virtual verkon myöntämä sivusto sivusto VPN-yhteys.

- Jos otat käyttöön Azure virtuaalikoneen ADFS-palvelimelle, yhteyden Windows Server Active Directory-toimialueen ohjaimet, määrite Stores ja määritys-tietokannat on tarpeen ja vaatia myös ExpressRoute tai sivuston sivuston VPN-yhteyden Azure virtual verkon ja paikallisen verkon välillä.

- Kaikki liikenne käytetään ovat peräisin Azuren näennäiskoneiden (juniin liikenne). Jos kustannus on ajo kerroin, on suositeltavaa ottaa Azure, jätä AD FS palvelinten paikallisen Web-sovelluksen välityspalvelimen solmut. Jos AD FS-palvelimet otetaan käyttöön myös Azuren näennäiskoneiden, lisäkustannuksia aiheutuvat tarkistamiseen Paikalliset käyttäjät. Juniin liikenne veloitetaan kustannus riippumatta siitä, onko se on sallita ohittaa ExpressRoute tai sivuston sivuston VPN-yhteyttä.

- Jos päätät käyttää Azure's alkuperäisen palvelimen kuormituksen suuren käytettävyyden AD FS-palvelimien ominaisuuksia, Huomaa, että kuormituksen tasaamisen tarjoaa keräysputkien, joita käytetään näennäiskoneiden cloud-palvelun terveydentilasta. Azuren näennäiskoneiden (eikä web- tai työntekijä roolit) mukautetut näytteenottimen on käytettävä jälkeen, joka vastaa oletusarvon keräysputkien agentti ei ole Azuren näennäiskoneiden. Yksinkertaisuuden, voit käyttää mukautettuja TCP-näytteenottimen — vain edellyttää, että TCP-yhteyttä (TCP SYN-segmentin lähetetty ja SYN-ACK TCP-segmentin palautti) onnistuneesti muodostaa virtuaalikoneen terveydentilasta. Voit määrittää mukautetun keräysputken käyttää mitä tahansa porttinumeroa, johon oman näennäiskoneiden aktiivisesti listening.

> [AZURE.NOTE] Koneet, jotka haluat näyttää samat portit suoraan Internetiin (esimerkiksi portti 80 ja 443) ei voi jakaa saman pilvipalvelussa. On-vuoksi suositeltavaa luoda erillinen pilvestä palvelu Windows Server AD FS-palvelimia siten vältät mahdollisuuksia päällekkäinen sovelluksen: n ja Windows Server AD FS välillä.

## <a name="deployment-scenarios"></a>Käyttöönottoskenaariot

Seuraavassa osassa kuvataan commonplace käyttöönoton skenaariot huomion otettavia asioita, jotka on otettava huomioon. Kunkin skenaarion on lisätietoja päätökset ja huomioon otettavia seikkoja kannattaa linkkejä.

1. [AD DS: AD DS-aware sovelluksen kanssa ei edellytä yrityksen verkkoyhteydessä ottaminen käyttöön](#BKMK_CloudOnly)

    Esimerkiksi Internetiin yhteydessä oleva SharePoint-palvelu on otettu käyttöön Azure virtuaalikoneen. Sovellus ei ole riippuvuuksia yrityksen verkkoon resursseja. Sovelluksen vaativat Windows Server AD DS, mutta ei edellytä yrityksen Windows Server AD DS.

2. [AD FS: Laajenna saatavat tukeva paikallisen edusta sovellus Internetiin](#BKMK_CloudOnlyFed)

    Esimerkiksi saatavat tukeva sovellus, jossa on otettu käyttöön onnistuneesti paikallisen ja yrityksen käyttäjät käyttävät on Ryhdy käyttämään Internetistä. Sovellus on voi käyttää suoraan Internetin välityksellä sekä oman yrityksen käyttäjätietojen käyttämisestä liikekumppaneita ja yrityksen aiemmin luoduille käyttäjille.

3. [AD DS: Ottaminen käyttöön Windows Server AD DS-aware sovellus, jossa vaativat yrityksen verkkoon](#BKMK_HybridExt)

    Esimerkiksi LDAP-aware sovellus, joka tukee Windows-integroitua todennusta ja käyttää Windows Server AD DS: N määritys ja käyttäjäprofiilien tietojen säilö on otettu käyttöön Azure virtuaalikoneen. On suositeltavaa ja hyödyntää olemassa olevan yrityksen Windows Server AD DS verovapautta toimittamalla single Sign-sovelluksen. Sovellus ei ole saatavat tarkistavan.

### <a name="BKMK_CloudOnly"></a>1. AD DS: AD DS-aware sovelluksen kanssa ei edellytä yrityksen verkkoyhteydessä ottaminen käyttöön

![Vain pilvipalveluita AD DS: N käyttöönoton](media/active-directory-deploying-ws-ad-guidelines/ADDS_cloud.png)
**Kuva 1**

#### <a name="description"></a>Kuvaus

SharePoint on otettu käyttöön Azure virtuaalikoneen ja sovellus ei ole riippuvuuksia yrityksen verkkoon resursseja. Sovelluksen vaativat Windows Server AD DS, mutta ei *ole* yrityksen Windows Server AD DS edellyttävät. Liitetyt luottamussuhteet eikä Kerberos tarvitaan, koska käyttäjät ovat itse valmistellun kautta Windows Server AD DS-toimialueeseen, joka on myös pilveen Azure-virtuaalikoneissa-sovellus.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Skenaario huomioon otettavia seikkoja ja miten tekniikka alueet Käytä skenaario

- [Verkkotopologia](#BKMK_NetworkTopology): paikallisen-connectivity (tunnetaan myös nimellä sivusto sivusto connectivity) ilman Azure virtual verkoston luominen.

- [Toimialueen Ohjauskoneen käyttöönoton määritys](#BKMK_DeploymentConfig): asentaa uusia toimialueen ohjauskoneen uusi, yhden toimialueen Windows Server Active Directory-metsää. Tämä käyttöön Windows DNS-palvelimen kanssa.

- [Windows Server Active Directory-sivuston topologian](#BKMK_ADSiteTopology): Käytä Windows Server Active Directory oletussivuston (kaikissa tietokoneissa on oletusarvoinen ensimmäisen-sivuston nimi).

- [IP-osoitteiden ja DNS](#BKMK_IPAddressDNS):

 - Määrittää staattinen IP-osoite Palvelinkeskus määrittäminen AzureStaticVNetIP Azure PowerShell cmdlet-komennolla.
 - Asenna ja määritä Windows Server DNS Azure toimialue-laitteen käyttöön.
 - Määritä nimi ja IP-osoite, joka isännöi Ohjauskoneen ja DNS-palvelinrooleista AM VPN-ominaisuudet.

- [Yleinen luettelo](#BKMK_GC): ensimmäisen Yhdysvalloissa sijaitsevaan Palvelinkeskukseen metsää on oltava Yleinen luettelo-palvelimeen. Muita ohjauskoneita on myös määritettävä kokoonpanojaan kuin koska yksi toimialue-metsää Yleinen luettelo ei edellytä mitään toimia-Palvelinkeskus.

- [Windows Server AD DS tietokannan ja SYSVOL sijaintia](#BKMK_PlaceDB): tietojen DVD-levyllä lisääminen käynnissä Azure VMs muodossa, jotta voit tallentaa Windows Server Active Directory-tietokantaan, lokit ja SYSVOL ohjauskoneita.

- [Varmuuskopiointi ja palauttaminen](#BKMK_BUR): määrittää, johon haluat tallentaa järjestelmän tilan varmuuskopiot. Tarvittaessa lisätä toimialueen Ohjauskoneen AM tallentaa varmuuskopioita tietojen toiselle levylle.

### <a name="BKMK_CloudOnlyFed"></a>2 AD FS: Laajenna saatavat tukeva paikallisen edusta sovellus Internetiin

![Paikallisen-yhteyttä Federation](media/active-directory-deploying-ws-ad-guidelines/Federation_xprem.png)
**Kuva 2**

#### <a name="description"></a>Kuvaus

Saatavat tukeva sovellus, jossa on otettu käyttöön onnistuneesti paikallisen ja yrityksen käyttäjät käyttävät on tullut käytettävissä suoraan Internetistä. Sovellus on web-edusta SQL-tietokantaan, johon se tallentaa tiedot. SQL-palvelimien sovellus käyttää myös sijaitsevat yrityksen verkkoon. Kaksi Windows Server AD FS STSs ja kuormituksen-on otettu käyttöön paikallisen yrityskäyttäjät voivat käsitellä. Nyt sovellus on lisäksi käyttää suoraan Internetin välityksellä sekä oman yrityksen käyttäjätietojen käyttämisestä liikekumppaneita ja yrityksen aiemmin luoduille käyttäjille.

Yksinkertaistamaan ja uusi tämä vaatimus käyttöönoton ja määrityksen tarpeiden yrittäessäsi, se on päättänyt, kaksi muuta verkko frontends ja kaksi Azuren näennäiskoneiden voi asentaa Windows Server AD FS-välityspalvelimet. Kaikki neljä VMs voi luovuttaa yhteys Internetiin, ja annetaan yhteys paikalliseen verkkoon Azure Virtual Network sivusto sivusto VPN-ominaisuutta.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Skenaario huomioon otettavia seikkoja ja miten tekniikka alueet Käytä skenaario

- [Verkkotopologia](#BKMK_NetworkTopology): Azure virtual verkon ja [määrittää paikallisen - yhteyden](../vpn-gateway/vpn-gateway-site-to-site-create.md)luominen.

 > [AZURE.NOTE] Kunkin Windows Server AD FS varmenteet-varmistaa, että sertifikaattimalli ja tuloksena olevat varmenteet määritetyssä URL-Osoitteen voi siirtyä sellaisen Azure käytössä Windows Server AD FS-esiintymät. Tämä saattaa edellyttää paikallisen-yhteyksien PKI infrastruktuurin osiin. Jos esimerkiksi jos luettelon päätepisteen on LDAP-pohjaisten ja pitäminen yksinomaan paikallisen, valitse rajat paikallisen connectivity on suoritettava. Jos tämä ei olisi, se saattaa olla tarpeen käyttämään varmenteita varmenteen myöntäjä, jonka luettelon pääsee Internetin välityksellä.

- [Cloud services-kokoonpanon määritys](#BKMK_CloudSvcConfig): sinulla on kaksi pilvipalveluihin järjestyksessä antaa kaksi kuormituksen virtual IP-osoitteet. Ensimmäinen pilvipalvelussa virtual IP-osoite ohjataan kaksi Windows Server AD FS välityspalvelimen VMs porttien 80 ja 443. Windows Server AD FS ‑välityspalvelin VMs määritetään osoittamaan paikallisen-kuormituksen IP-osoite, etupuoli Windows Server AD FS-STSs. Toinen pilvipalvelussa virtual IP-osoite ohjataan kaksi VMs käytössä web-edusta uudelleen portit 80 ja 443. Määritä mukautettu näytteenottimen varmistamiseksi kuormituksen-ohjaa vain-liikenne paikalliseen toimivat Windows Server AD FS välityspalvelin ja web frontend VMs.

- [Federation server-määritys](#BKMK_FedSrvConfig): Windows Server määrittää AD FS kuin liittoutumispalvelimen (STS) suojaus tunnusten varten luotu pilveen Windows Server Active Directory-metsää luomiseen. Määritä federation tarjoajan luottamussuhteet vaateita, jotka haluat hyväksyä käyttäjätiedot-eri kumppaneiden kanssa ja määrittää varmenteen käyttäjän osapuolen luottamussuhteet tunnuksia, voit luoda eri sovellusten kanssa.

    Useimmissa tapauksissa Windows Server AD FS-välityspalvelimet otetaan käyttöön Internetiin yhteydessä oleva-kapasiteetti tietoturvasyistä, vaikka niiden Windows Server AD FS federation vastineiden pysyvät eristetty suora Internet-yhteys. Riippumatta siitä, että käyttöönottotapa sinun on määritettävä pilvipalvelussa sijaitsevaan virtual IP-osoite, joka tarjoaa julkisesti näkyviä IP-osoite ja portti, joka pystyy kuormituksen yli kaksi Windows Server AD FS STS esiintymää tai välityspalvelimen esiintymät.

- [Windows Server AD FS suuren käytettävyyden määritykset](#BKMK_ADFSHighAvail): on suositeltavaa ottaa käyttöön Windows Server AD FS-klusteriin on vähintään kaksi automaattisesti palvelimeen ja kuormituksen tasaus. Ehkä käyttämällä Windows sisäinen tietokanta (WID), Windows Server AD FS määritystietoja ja Azure sisäinen kuormituksen Vastatilin ominaisuutta avulla voit jakaa pyynnöt klusterin palvelinten.

Lisätietoja on artikkelissa [AD DS: N käyttöönotto-oppaassa](https://technet.microsoft.com/library/cc753963).


### <a name="BKMK_HybridExt"></a>3. AD DS: Ottaminen käyttöön Windows Server AD DS-aware sovelluksen, jotka vaativat yrityksen verkkoon

![Paikallisen-AD DS: N käyttöönoton](media/active-directory-deploying-ws-ad-guidelines/ADDS_xprem.png)
**Kuva 3**

#### <a name="description"></a>Kuvaus

LDAP-aware sovellus on otettu käyttöön Azure virtuaalikoneen. Se tukee Windows-integroitua todennusta ja käyttää Windows Server AD DS: N määrittäminen ja käyttäjän profiilitiedot säilö. Tavoitteena on hyödyntää olemassa olevan yrityksen Windows Server-AD DS ja anna kertakirjautumisen sovelluksen. Sovellus ei ole saatavat tarkistavan. Käyttäjien tarvitsee myös käyttää sovellusta suoraan Internetistä. Optimoi suorituskyvyn ja kustannukset, määritetään kahden toimialueen ohjauskoneen, jotka ovat osa yrityksen toimialueen otetaan käyttöön Azure ohjelman rinnalla.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Skenaario huomioon otettavia seikkoja ja miten tekniikka alueet Käytä skenaario

- [Verkkotopologia](#BKMK_NetworkTopology): luominen Azure virtual verkon [paikallisen välinen yhteys](../vpn-gateway/vpn-gateway-site-to-site-create.md).

- [Asennustapa](#BKMK_InstallMethod): yrityksen Windows Server Active Directory-toimialueen ohjauskoneita replikan käyttöönotto. Replikan Ohjauskoneen Asenna Windows Server AD DS AM ja voit myös asentaa-Media (IFM)-toiminnon avulla voit vähentää tiedot, jotka on replikoida uuden Ohjauskoneen asennuksen aikana. Katso opetusohjelma, [Asenna replikan Active Directory-ohjauskoneen Azure](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Vaikka käyttäisit IFM, se voi olla tehokkaampaa voivat laatia virtual Ohjauskoneen paikallisen ja siirtyminen koko Virtual kiintolevyn (Näennäiskiintolevyn) pilveen sijaan replikoiminen Windows Server AD DS: N asennuksen aikana. Turvallisuus on suositeltavaa, että poistat Näennäiskiintolevyn paikalliseen verkkoon, kun se on kopioitu Azure.

- [Windows Server Active Directory-sivuston topologian](#BKMK_ADSiteTopology): Azure uuden sivuston luominen Active Directory-sivustot ja -palvelut. Luo Windows Server Active Directory-aliverkon objektin esittämään Azure virtual verkon ja aliverkon lisääminen sivustoon. Luo uusi sivusto-linkissä, joka sisältää uuden Azure-sivuston ja sivuston, jossa Azure virtual verkon VPN päätepisteen sijaitsee hallinta ja Windows Server Active Directory-liikenne ja sieltä pois Azure optimointi.

- [IP-osoitteiden ja DNS](#BKMK_IPAddressDNS):

 - Määrittää staattinen IP-osoite Palvelinkeskus määrittäminen AzureStaticVNetIP Azure PowerShell cmdlet-komennolla.
 - Asenna ja määritä Windows Server DNS Azure toimialue-laitteen käyttöön.
 - Määritä nimi ja IP-osoite, joka isännöi Ohjauskoneen ja DNS-palvelinrooleista AM VPN-ominaisuudet.

- [GEO distributed ohjauskoneita](#BKMK_DistributedDCs): määrittää muita virtual verkot tarpeen mukaan. Jos Active Directory-sivuston topologian edellyttää paikkojen, jotka vastaavat eri Azure alueiden kuin haluat luoda Active Directory-sivustojen vastaavasti ohjauskoneita.

- [Vain luku - ohjauskoneet](#BKMK_RODC): Ohjauskoneen Azure-sivuston voi ottaa käyttöön, sen mukaan, tekemistä varten tarpeen kirjoittaminen vastaan Palvelinkeskus toimintojen ja sovellusten ja palveluiden yhteensopivuuden RODCs sivustoon. Lisätietoja sovellusten yhteensopivuudesta on [vain luku - toimialueen ohjaimet sovelluksen yhteensopivuuden opas](https://technet.microsoft.com/library/cc755190).

- [Yleinen luettelo](#BKMK_GC): kokoonpanojaan tarvitaan palvelun kirjautumispyynnöt ryhmätilien metsien. Älä ota yleistä Azure-sivustossa, jos maksamaan juniin liikenne kustannukset, sillä käyttöoikeuksien aiheuttaa kokoonpanojaan kyselyt muihin sivustoihin. Pienennä liikenne, voit ottaa yleinen ryhmän jäsenyyden välimuisti Azure Active Directory-sivustot ja -palvelut-sivuston.

    Jos otat käyttöön yleistä, Määritä linkit ja sivuston linkkien kustannukset niin, että Azure-sivuston luettelo ei ole ensisijainen lähteenä Ohjauskoneen tarvitsevat samaan osittaisen toimialueen osiot replikoida muihin kokoonpanojaan mukaan.

- [Windows Server AD DS tietokannan ja SYSVOL sijaintia](#BKMK_PlaceDB): tietojen DVD-levyllä lisääminen ohjauskoneita, jotta voit tallentaa Windows Server Active Directory-tietokantaan, lokit ja SYSVOL Azure VMs käytössä.

- [Varmuuskopiointi ja palauttaminen](#BKMK_BUR): määrittää, johon haluat tallentaa järjestelmän tilan varmuuskopiot. Tarvittaessa lisätä toimialueen Ohjauskoneen AM tallentaa varmuuskopioita tietojen toiselle levylle.

## <a name="deployment-decisions-and-factors"></a>Käyttöönoton päätökset ja huomioon otettavia seikkoja

Tässä taulukossa on yhteenveto Windows Server Active Directory-tekniikkaa alueet, jotka ovat vaikuttaneet edellä kuvattujen tilanteiden ja vastaavan päätöksiä huomioitavia tarkemmin alla olevia linkkejä. Tekniikka joidenkin alueiden ei ehkä koske jokaisen käyttöönottotapa ja tekniikka joidenkin alueiden ehkä Lisää ehdottoman tärkeää käyttöönoton tilanne tekniikka muualla kuin.

Esimerkiksi jos käyttöönottoa replikan Ohjauskoneen virtual verkossa ja että metsää on vain yksi toimialue, valitsemalla Yleinen luettelo Serverin käyttöönotto tässä tapauksessa eivät ole ehdottoman tärkeää käyttöönottotapa koska se ei luoda lisää replikoinnin vaatimuksia. Toisaalta, jos metsää on useita toimialueita, valitse Yleinen luettelo virtual verkon käyttöön päätös voi vaikuttaa kaistanleveyttä, suorituskyvyn, todennus, directory haut ja niin edelleen.

| Windows Server Active Directory-tekniikka-alue | Päätökset | Huomioon otettavia seikkoja |
| ---- | ---- | ---- |
| [Verkkotopologia](#BKMK_NetworkTopology) | Voit luoda virtuaalisen verkon? | <li>Corp resurssien käytön vaatimukset</li> <li>Todennus</li> <li>Tilinhallinta</li> |
| [Toimialueen Ohjauskoneen käyttöönoton määritys](#BKMK_DeploymentConfig) | <li>Ottaa minkä tahansa luottamussuhteet ilman erillistä metsää?</li> <li>Ottaa käyttöön new-metsää liittoutumistoiminnossa?</li> <li>Ottaa käyttöön new-metsää Windows Server Active Directory metsää luottamuksen tai Kerberos?</li> <li>Laajenna Corp metsää ottamalla replikan Ohjauskoneen?</li> <li>Laajenna Corp metsää ottamalla käyttöön uuden alitoimialueen tai toimialuepuun?</li> | <li>Tietoturva</li> <li>Yhteensopivuus</li> <li>Kustannukset</li> <li>Vikasietoisuudesta ja vikasietoa</li> <li>Sovellusten yhteensopivuuden</li> |
| [Windows Server Active Directory-sivuston topologian](#BKMK_ADSiteTopology) | Kuinka määrität aliverkosta, sivustojen ja linkkejä Azure Virtual verkon voit optimoida liikenteen ja minimoida kustannukset kanssa? | <li>Aliverkon ja sivuston määritelmät</li> <li>Sivuston Tietolinkin ominaisuudet ja muuta ilmoitus</li> <li>Replikoinnin pakkaus</li> |
| [IP-osoitteiden ja DNS: STÄ](#BKMK_IPAddressDNS) | Määrittämisestä IP-osoitteet ja nimenselvitys toimii? | <li>Käytä Set-AzureStaticVNetIP cmdlet-komennon avulla voit määrittää staattinen IP-osoite</li> <li>Asenna Windows Server DNS-palvelin ja nimi ja IP-osoite, joka isännöi Ohjauskoneen ja DNS-palvelinrooleista AM VPN-ominaisuuksien määrittäminen</li> |
| [GEO distributed ohjauskoneita](#BKMK_DistributedDCs) | Miten replikoida ohjauskoneissa erillisessä virtual verkkoja? | Jos Active Directory-sivuston topologian edellyttää paikkojen, joka vastaa eri Azure alueiden kuin haluat luoda Active Directory-sivustojen vastaavasti ohjauskoneita. [Määritä virtual verkon virtual verkkoon Connectivity](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) replikoida toimialueen ohjainten erillisessä virtual verkoissa välille. |
| [Vain luku-ohjauskoneet](#BKMK_RODC) | Vain luku- tai kirjoitettavat ohjauskoneita käyttäminen | <li>Suodattaa HBI/PII määritteet</li> <li>Suodattaa tietoja</li> <li>Raja lähtevän tietoliikenteen</li> |
| [Yleinen luettelo](#BKMK_GC) | Asenna Yleinen luettelo? | <li>Varmista kaikki ohjauskoneita kokoonpanojaan yhden toimialueen metsää</li> <li>Ryhmätilien metsää, kokoonpanojaan vaaditaan todennusta varten</li> |
| [Asennustapa](#BKMK_InstallMethod) | Voit asentaa Ohjauskoneen Azure? | Joko: <li>Asenna Windows PowerShell-tai Dcpromo AD DS: ÄÄN</li> <li>Siirrä paikallisen virtual-toimialueen Ohjauskoneen Näennäiskiintolevyn</li> |
| [Windows Server AD DS tietokannan ja SYSVOL sijaintia](#BKMK_PlaceDB) | Mistä tallentamiseen Windows Server AD DS tietokannan lokit ja SYSVOL? | Muuta Dcpromo.exe oletusarvoja. Nämä kriittinen Active Directory tiedostot *on* asetettava Azure tietojen levyille käyttöjärjestelmän levyjä, jotka toteuttavat kirjoittaminen välimuistiin sijaan. |
| [Varmuuskopiointi ja palauttaminen](#BKMK_BUR) | Voit suojata ja palauttaa tietoja? | Järjestelmän tilan varmuuskopiot luominen |
| [Federation server-määritys](#BKMK_FedSrvConfig) | <li>Ottaa käyttöön new-metsää liittoutumistoiminnossa pilveen?</li> <li>AD FS paikalliseen käyttöön ja näyttää välityspalvelimen pilvessä?</li> | <li>Tietoturva</li> <li>Yhteensopivuus</li> <li>Kustannukset</li> <li>Liikekumppanien-sovellukset</li> |
| [Cloud services-kokoonpanon määritys](#BKMK_CloudSvcConfig) | Pilvipalveluun implisiittisesti on otettu käyttöön ensimmäistä kertaa, voit luoda virtual machine. Tarvitsetko Lisää pilvipalveluihin otetaan käyttöön? | <li>AM tai VMs edellyttävät Internet suora näyttäminen?</li> <li> Tarvitseeko palvelun kuormituksen tasaamisen?</li> |
| [Liitetty viestintä palvelinvaatimukset julkisista ja yksityisistä IP-osoitteet (dynaaminen IP ja virtual IP)](#BKMK_FedReqVIPDIP) | <li>Tarvitseeko Windows Server AD FS-esiintymä voi siirtyä suoraan Internetistä?</li> <li>Tarvitseeko sovellus otetaan käyttöön pilvessä omassa Internetiin yhteydessä oleva IP-osoite ja portin?</li> | Luoda yksi-pilvipalvelussa kunkin virtual IP-osoite, joka on vaatii |
| [Windows Server AD FS suuren käytettävyyden määrittäminen](#BKMK_ADFSHighAvail) | <li>Windows Server AD FS Omat palvelinklusterin näkyvien solmujen?</li> <li>Kuinka monta solmujen Omat Windows Server AD FS-välityspalvelimen klusterin käyttöön?</li> | Vikasietoisuudesta ja vika poikkeama |

### <a name="BKMK_NetworkTopology"></a>Verkkotopologia

Windows Server AD DS vaatimukset DNS ja IP-osoite yhdenmukaisuuden saavuttamiseksi on ensin [Azure virtual verkon](../virtual-network/virtual-networks-overview.md) luominen ja lisääminen näennäiskoneiden liittäminen. Sen luomisen aikana voit päättää, voit myös laajentaa yhteys paikalliseen yrityksen verkkoon, joka yhdistää läpinäkyvä Azuren näennäiskoneiden paikallisen koneet — Tämä saavutetaan käyttöä perinteinen VPN-toiminnot ja edellyttää, että VPN-päätepisteen voi luovuttaa reunan yrityksen verkkoon. Toisin sanoen VPN-yhteys on käynnistetty Azure yrityksen verkkoon, ei päinvastoin.

Huomaa, että muut ovat voimassa, kun laajentaminen virtual verkon lisäksi vakio maksut, jotka koskevat kunkin AM paikalliseen verkkoon. Tarkemmin sanottuna on kulujen Azure Virtual Network yhdyskäytävän suorittimen kerran ja kunkin AM, joka on yhteydessä paikallisen koneet VPN-yhteyden luoma juniin tietoliikenteen. Lisätietoja verkon liikenne ovat artikkelissa [Azure hinnat osoitteessa--nopeasti](http://azure.microsoft.com/pricing/).

### <a name="BKMK_DeploymentConfig"></a>Toimialueen Ohjauskoneen käyttöönoton määritys

Tapa, jolla voit määrittää Palvelinkeskus määräytyy haluat suorittaa Azure-palvelu vaatimukset. Esimerkiksi uusi metsää erillään oman yrityksen metsää testikäyttöön, käsitteiden, uuden sovelluksen tai joitakin muita lyhyen aikavälin projektin, joka edellyttää hakemistopalvelujen, mutta eivät tiettyjä sisäinen yrityksen resurssien käytön voi ottaa käyttöön.

Etu-muodossa erillään metsää Ohjauskoneen ei replikoida kanssa paikalliset ohjauskoneita, tuloksena on vähemmän lähtevän verkkoliikenteen itse, järjestelmä luo pienentää suoraan kustannuksia. Lisätietoja verkon liikenne ovat artikkelissa [Azure hinnat osoitteessa--nopeasti](http://azure.microsoft.com/pricing/).

Toinen esimerkki oletetaan on palvelu tietosuojaa koskevat vaatimukset, mutta palvelun määräytyy sisäinen Windows Server Active Directory käyttöoikeuksien. Ovat sallii host tietoihin palvelun pilveen, lapsen verkkotunnuksen ehkä käyttöönottaminen oman sisäinen metsää Azure. Tässä tapauksessa voit ottaa toimialueen Ohjauskoneen, uusi alitoimialueen (ilman avulla osoite yksityisyys Yleinen luettelo). Tässä skenaariossa sekä replikan Ohjauskoneen käyttöönoton edellyttää virtual verkon paikallisen ohjauskoneita käyttävää yhteyttä.

Jos luot uutta metsää, valitse, haluatko käyttää [Active Directory luottaa](https://technet.microsoft.com/library/cc771397) tai [Federation luottaa](https://technet.microsoft.com/library/dd807036). Saldo vaatimukset, yhteensopivuuden, tietoturva, vaatimustenmukaisuus, kustannukset ja vikasietoisuudelle mukaisesti. Esimerkiksi [Valikoiva todennus](https://technet.microsoft.com/library/cc755844) hyödyntää voit halutessasi ottaa käyttöön new-metsää Azure- ja Windows Server Active Directory luottamuksen paikallisen metsää ja cloud metsää välillä. Jos sovellus on saatavat tietää, mutta voi käyttöönottoa sijaan metsää luottamussuhteet Active Directory federation luottamussuhteet. Toinen tekijä on replikoida lisätietojen pidentämällä paikallisen Windows Server Active Directory pilveen tai Luo Lisää lähtevän tietoliikenteen todennus ja kyselyn kuormituksen kustannukset.

Käytettävyys ja vika poikkeama vaatimukset vaikuttaa myös valintasi. Esimerkiksi jos linkki katkeaa, sovellusten hyödyntäminen Kerberos-luottamusta tai liitetty viestintä luota ovat kaikki todennäköisesti kokonaan jaettu paitsi, jos olet ottanut riittävän infrastruktuurin Azure. Vaihtoehtoinen käyttöönoton määrityksiä kuten replikan ohjauskoneita (kirjoitettavat tai RODCs) Suurenna todennäköisyys, että voit Hyväksy linkki katkokset.

### <a name="BKMK_ADSiteTopology"></a>Windows Server Active Directory-sivuston topologian

Sinun täytyy määrittää oikein sivustot ja linkkejä haluat optimoida liikenteen ja minimoida kustannukset. Sivustojen, sivuston linkkien ja aliverkosta vaikuttavat replikoinnin topologian ohjauskoneita ja tietoliikenteen kulun välillä. Ota huomioon seuraavat liikenne maksut ja ottaminen käyttöön ja määrittäminen ohjauskoneita käyttöönotto-skenaario vaatimusten mukainen:

- Ei korko.vuosi maksu tunnissa yhdyskäytävän itse:

 - Ja pysäytetty tarpeen mukaan

 - Jos pysäytetty, Azure VMs eristetään yrityksen verkosta

- Saapuvan liikenteen on ilmainen

- Lähtevän tietoliikenteen veloitetaan mukaan [Azure hinnoittelua osoitteessa--nopeasti](http://azure.microsoft.com/pricing/). Voit optimoida sivuston Tietolinkin ominaisuudet paikallisen sivustot ja cloud-sivustojen välillä seuraavasti:

 - Jos käytössäsi on useita verkkoja virtual, Määritä sivustolinkkejä- ja niiden kustannuksista asianmukaisesti priorisointi Azure sivuston päälle, joka voi toimittaa samat käyttöoikeustasot palvelun maksutta Windows Server AD DS estää. Kannattaa myös siltaan painikkeen (joka on oletusarvoisesti käytössä) linkki (BASL)-asetus käytöstä. Näin varmistat, että vain suoraan yhdistettyjä sivustojen replikoida toisiinsa. Ohjauskoneita transitiivisesti yhdistetyn sivustot eivät ole enää voi replikoida suoraan toistensa kanssa, mutta se on replikoida Yleiset- sivuston kautta. Jos välittävät sivustojen poistuvat käytöstä jostain syystä, replikointi ohjauskoneita transitiivisesti yhdistetyn sivustojen välillä ei tehdä, vaikka sivustojen välinen yhteys on käytettävissä. Lopuksi transitiiviverbien replikoinnin käyttäytymisen osat säilyvät olisi, jossa sivuston luominen linkki siltoja, jotka sisältävät haluamasi sivustolinkkejä ja sivustoissa, kuten paikallisen, yritysverkon sivustot.

 - [Määritä sivusto kustannuksia](https://technet.microsoft.com/library/cc794882) oikein voit välttää tahattomia liikenne. Jos **Yritä seuraavan lähinnä sivusto** -asetus on käytössä, varmista, VPN-sivustot eivät ole seuraava lähimpänä suurentamalla sivustolinkin objektin, joka yhdistää Azure sivuston yritysverkon liittyvät kustannukset.

 - Määritä sivuston linkkiä [välit](https://technet.microsoft.com/library/cc794878) ja [aikatauluja](https://technet.microsoft.com/library/cc816906) yhdenmukaisuuden vaatimukset ja objekti muuttuu suuruuden mukaan. Tasaa replikoinnin aikataulu viive tarkkuudella. Ohjauskoneita replikoida vain viimeisimmän tilan arvoa, jotta pienenevillä replikoinnin välin tallentaa kustannukset on riittävä objektin muuttaminen korko.

- Jos pienentäminen kustannukset on prioriteetti, varmista replikoinnin ajoitetaan ja muuta ilmoitus ei ole otettu käyttöön. Tämä on oletusarvo-määritykset, kun replikoiminen sivustojen välillä. Tämä ei ole merkitystä, jos otat Ohjauskoneen virtual verkon koska Ohjauskoneen ei replikoida lähtevä muutokset. Mutta jos otat käyttöön kirjoitettava Ohjauskoneen, varmista että sivustolinkin ei ole määritetty replikointia päivitykset tarpeettomat usein. Jos otat käyttöön Yleinen luettelo-palvelimen yleisen (luettelon), varmista, että jokaisen sivustoa, joka sisältää yleistä kopioi toimialueen osioiden lähteestä Ohjauskoneen sivustoon, jossa on liitetty sivuston linkkiä tai sivuston – linkit, joka on pienempi kuin luettelo kustannus Azure-sivustossa.


- On mahdollista vähentää edelleen edelleen verkkoliikennettä luoma replikoinnin muuttamalla replikoinnin pakkaamisen algoritmin sivustojen välillä. Tiivistys-algoritmin ohjataan REG_DWORD rekisterin tapahtuma HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Replicator pakkaamisen algoritmin. Oletusarvo on 3, joka numeroidulla Xpress pakkaa algoritmin. Voit muuttaa arvon 2, joka muuttuu algoritmin MSZip. Useimmissa tapauksissa tämä kasvattaa Tiivistys, mutta se tekee kustannuksella suorittimen käyttö. Lisätietoja on artikkelissa [miten Active Directoryn replikoinnin topologian toiminta](https://technet.microsoft.com/library/cc755994).

### <a name="BKMK_IPAddressDNS"></a>IP-osoitteiden ja DNS: STÄ

Azuren näennäiskoneiden on varattu "DHCP vuokratun osoitteet" oletusarvoisesti. Dynaaminen Azure virtual verkko-osoitteiden jatkuvat virtuaalikoneen elinkaaren virtual tietokoneen kanssa, koska Windows Server AD DS vaatimukset täyttyvät.

Tuloksena käytettäessä dynaamisen osoitteen Azure-sallitaan käyttämällä staattinen IP-osoite, koska se on reititettävä ajan, jolloin ja ajan, jolloin on yhtä kuin pilvipalvelussa sijaitsevaan elinkaaren.

Dynaaminen osoite on kuitenkin Varaus poistettu Jos AM suljetaan. Jos haluat estää IP-osoite varaus on poistettu, voit [käyttää Set-AzureStaticVNetIP staattinen IP-osoitteiden](http://social.technet.microsoft.com/wiki/contents/articles/23447.how-to-assign-a-private-static-ip-to-an-azure-vm.aspx).

Nimenselvitys, saat käyttöön oman (tai hyödyntää nykyinen) DNS server-julkaisuinfrastruktuuri; Antamalla Azure DNS ei vastannut Windows Server AD DS Lisäasetukset nimi tarkkuus tarpeisiin. Esimerkiksi se ei tue dynaaminen SRV-tietueet ja niin edelleen. Nimenselvitys on ohjauskoneita ja toimialueen liittyneet asiakkaiden kokoonpanon kohde. Ohjauskoneet on pystyttävä rekisteröiminen tietueiden ja muiden Ohjauskoneen tietueiden ratkaiseminen.
Poikkeama ja suorituskyvyn syistä vika on tallenteen Windows Server DNS-palvelun asentaminen ohjauskoneet Azure-käyttöjärjestelmässä. Valitse Azure virtual verkko-ominaisuuksien määrittäminen nimi ja IP-osoite, DNS-palvelimen kanssa. Kun muut VMs virtual verkossa, niiden DNS asiakasasetukset tulkintatoiminnon on määritetty osana dynaaminen IP-osoite kohdistus DNS-palvelimeen.

> [AZURE.NOTE] Et voi liittyä paikalliseen tietokoneeseen Windows Server AD DS Active Directory-toimialueen, joka sijaitsee Azure suoraan Internetin välityksellä. Portti-vaatimukset, Active Directory ja toimialueen liittyminen näkyvät sen hankalaa suoraan näyttää tarvittavat portit ja käytössä koko Ohjauskoneen, Internetiin.

VMs rekisteröi DNS nimensä automaattisesti käynnistettäessä tai kun nimen muuttaminen.

Katso lisätietoja tässä esimerkissä ja toinen esimerkki, jossa näkyvät ensimmäisen AM valmistelu ja AD DS: N asentaminen [Asenna uusi Active Directory-metsää Microsoft Azure](active-directory-new-forest-virtual-machine.md). Saat lisätietoja Windows PowerShellin [Azure PowerShellin asentaminen](../powershell-install-configure.md) ja [Azure hallinnan cmdlet-komennot](https://msdn.microsoft.com/library/azure/jj152841).

### <a name="BKMK_DistributedDCs"></a>GEO distributed ohjauskoneita

Azure tarjoaa etuja, kun useita eri virtual verkoissa ohjauskoneita isännöinnin:

- Usean sivuston vikasietoa

- Fyysinen lähellä toimipisteiden (pienet viive)

Lisätietoja suora viestintä virtual verkkojen välillä määrittämisestä on artikkelissa [VPN-yhteyden määrittäminen virtual verkon](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

### <a name="BKMK_RODC"></a>Vain luku-ohjauskoneet

Sinun täytyy valita, haluatko ottaa käyttöön vain luku- tai kirjoitettavat ohjauskoneita. Olet ehkä kulmassa ottamaan RODCs, koska et voi valita fyysinen päättää, mutta RODCs on suunniteltu vastuullasi, kuten toimipisteiden niiden fyysinen turvallisuus missä sijainneissa käyttöön.

Azure ei ole fyysinen tietoturvariskin Tampereen toimistossa, mutta RODCs voi edelleen todista olevan kustannusten tehokkaiden, koska ne sisältävät ominaisuudet ovat hyvin hyödyntää näissä ympäristöissä, vaikka hyvin eri syistä. Esimerkiksi RODCs lähtevä replikointia ei ole ja voivat täyttää valikoivasti tietoja (salasanat). Entä huonot puolet näitä tietoja puuttuminen voi edellyttää tarvittaessa lähtevän tietoliikenteen vahvistamiseen käyttäjänä tai tietokoneen todentaa. Mutta tietoja voi valikoivasti kopioidaan ja välimuistissa.

RODCs antaa muita etu- ja HBI ja henkilökohtaisia tietoja koskee, koska voit lisätä määritteitä, jotka sisältävät luottamuksellisia tietoja Ohjauskoneen suodatettu määrite (KO). KO on mukautettavia attribuuteista, jotka ovat ei replikoida RODCs. Voit käyttää KO suojaavat siltä varalta, ei sallita, tai et halua tallentaa henkilökohtaisia tietoja tai HBI Azure. Lisätietoja on artikkelissa [Ohjauskoneen suodatetut määrite [(https://technet.microsoft.com/library/cc753459)].

Varmista, että sovellukset on yhteensopiva RODCs aiot käyttää. Monet käytössä Windows Server Active Directory-sovellukset toimivat hyvin RODCs, mutta jotkin sovellukset voivat suorittaa tehottomasti tai epäonnistuu, jos heillä ei on kirjoitettava Ohjauskoneen käyttöoikeus. Lisätietoja on [vain luku-ohjauskoneet sovelluksen](https://technet.microsoft.com/library/cc755190)-yhteensopivuus oppaassa.

### <a name="BKMK_GC"></a>Yleinen luettelo

Sinun täytyy asentaa yleinen luetteloon. Yksittäisen toimialue-metsää-kannattaa määrittää kaikki ohjauskoneita kuin Yleinen luettelo-palvelimiin. Se ei ole kasvattaa kustannukset, koska ei ole muita replikointia ei liikenne.

Ryhmätilien metsää Laajenna Universal ryhmän jäsenyyksiä aikana tarvitaan kokoonpanojaan. Jos Älä ota yleistä toiminnoista virtual verkossa, valitse Azure Ohjauskoneen todennetaan epäsuorasti luo lähtevän tietoliikenteen kysely kokoonpanojaan paikallisen todennus jokaisen yrityksen aikana.

Kokoonpanojaan liittyvät kustannukset ovat ennakoitavissa pienempi, koska ne isännöidä jokaisen toimialueen (n-osa). Jos työmäärää isännöi Internetiin yhteydessä oleva-palvelua ja todentaa käyttäjät Windows Server AD DS vastaan, kustannukset voivat olla täysin odottamattomia. Pakettitiedoston yleisen luettelon kyselyjen cloud-sivuston ulkopuolella todennuksen aikana, voit [ottaa käyttöön Universal ryhmän jäsenyyden välimuisti](https://technet.microsoft.com/library/cc816928).

### <a name="BKMK_InstallMethod"></a>Asennustapa

Sinun täytyy valita asentaminen ohjauskoneet virtual verkossa:

- Voit viedä uusi ohjauskoneita. Lisätietoja on artikkelissa [Asenna uusi Active Directory-metsää Azure virtual verkossa](active-directory-new-forest-virtual-machine.md).

- Siirtyminen pilveen paikallisen virtual-toimialueen Ohjauskoneen Näennäiskiintolevyn. Tässä tapauksessa sinun on varmistettava, että paikallisen virtual Ohjauskoneen "siirretään," ei "kopioitu" tai "kloonatun."

Käytä vain Azuren näennäiskoneiden ohjauskoneita (eikä Azure "WWW" tai "työntekijä" rooli VMs). Ne ovat kestävät ja toimialueen Ohjauskoneen osavaltio kestävyys vaaditaan. Azuren näennäiskoneiden on suunniteltu toiminnoista, kuten ohjauskoneita.

Älä käytä SYSPREP ottaminen käyttöön tai Kloonaa ohjauskoneita. Mahdollisuus Kloonaa ohjauskoneita on vain käytettävissä alkua Windows Server 2012: ssa. Kloonausohjelmistoja ominaisuus edellyttää tuki VMGenerationID pohjana Hypervisorissa. Windows Server 2012 ja Azure virtual verkoissa Hyper-V tukevat molemmat VMGenerationID, kolmannen osapuolen virtualization toimittajat tavalla.

### <a name="BKMK_PlaceDB"></a>Windows Server AD DS tietokannan ja SYSVOL sijaintia

Valitse Etsi Windows Server AD DS tietokannan, lokit ja SYSVOL. Ne on otettava käyttöön levyille Azure-tietoihin.

> [AZURE.NOTE] 1 TT on rajoitettu Azure tietojen levyjä.

Tietoja levyasemien tehdä välimuistin kirjoituksia oletusarvoisesti. Tiedot, jotka on liitetty AM levyasemien käyttää yksisuuntaisen kirjoituksen välimuistia. Yksisuuntaisen kirjoituksen välimuistin tekee että kirjoitus on sitoutunut kestävät Azure-tallennustilan, ennen kuin tapahtuma on valmis AM käyttöjärjestelmän näkökulmasta. Se sisältää kestävyyttä kustannuksella hieman hitaammin kirjoituksia.

Tämä on tärkeää Windows Server AD DS, koska kirjoittaminen ohjattavaa levy-välimuistin kumoavat tekemät Palvelinkeskus perustiedot. Windows Server AD DS yrittää poistaa käytöstä kirjoituksen välimuistiin, mutta se on enintään levyn IO järjestelmän vastaamaan sitä. Virhe kirjoittaminen välimuistiin tallentamisen poistaminen käytöstä voi tietyissä tilanteissa esitellä USN palauttaminen tuloksena on lingering objektit ja muut ongelmat.

Virtual ohjauskoneita parhaat käytännöt toimi seuraavasti:

- Määrittää Host välimuistin kieliasetus Azure tietojen levyn ei mitään. Tällöin kirjoittaminen välimuistiasetukset AD DS: N toiminnot liittyvät ongelmat.

- Tallentaa tietokannan, lokit ja SYSVOL joko samat tiedot levyn tai erillinen tietojen levyjä. Tämä on yleensä itse käyttöjärjestelmä, jota käytetään levyltä erillinen levy. Avaimen takeaway on, että Windows Server AD DS tietokannan ja SYSVOL on ei tallenneta Azure-käyttöjärjestelmän-levyn tyyppi. AD DS: N asennusprosessi asentaa oletusarvoisesti komponentit % järjestelmän pääkansio % kansioon, joka ei ole suositeltavaa Azure.

### <a name="BKMK_BUR"></a>Varmuuskopiointi ja palauttaminen

Huomioon mitä on, ei tueta varmuuskopiointia ja palauttamista Ohjauskoneen Yleinen ja tarkemmin, ne AM käynnissä. Katso [Varmuuskopiointi ja palauttaminen Huomioitavaa virtualisoidussa ohjauskoneita](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv#backup_and_restore_considerations_for_virtualized_domain_controllers).

Luo järjestelmän tilan varmuuskopiot käyttämällä vain varmuuskopiointi ohjelmia, jotka on nimenomaisesti tietoinen Windows Server AD DS, kuten Windows Server varmuuskopiointi varmuuskopiointi vaatimukset.

Älä kopioi tai Kloonaa näennäiskiintolevytiedostoja sitten välin sijaan suorittamiseen säännöllisen varmuuskopioinnin suunnittelu. Palautuksen koskaan olisi yhteystietokorttiisi käyttämällä kloonatun tai kopioitu näennäiskiintolevyjen ilman Windows Server 2012 ja tuettujen hypervisor esittelee USN Kuplat.

### <a name="BKMK_FedSrvConfig"></a>Federation server-määritys

Windows Server AD FS-liittoutumispalvelimia (STSs) määritys määräytyy osittain täytyykö sovellukset, jotka haluat ottaa käyttöön Azure resursseihin paikallisen verkossa.

Jos seuraavat ehdot täyttyvät sovellukset, sovellusten yksinään voi ottaa käyttöön paikallisen verkosta.

- Ne Hyväksy SAML suojauksen tunnusten

- Ne ovat exposable Internetiin

- Hän ei käyttää paikallisen resurssit

Määritä tässä tapauksessa Windows Server AD FS STSs seuraavasti:

1. Määritä erillään yhden toimialueen-metsää Azure.

2. Tiettyjen liitetyt käyttö metsää määrittämällä Windows Server AD FS-liittoutumispalvelinten palvelinfarmiin.

3. Määrittää paikallisen metsää Windows Server AD FS (federation palvelinklusterin ja liittoutumispalvelimen välityspalvelimen palvelinklusterin).

4. Muodostaa federation luottamussuhde paikallisen ja Windows Server AD FS Azure esiintymien välillä.

Toisaalta, jos sovellukset edellyttävät paikallisen resurssien käytön, saattoi Windows Server AD FS Azure-sovellukseen seuraavasti:

1. Määrittää paikallisen verkkojen ja Azure väliset yhteydet.

2. Määrittää paikallisen metsää Windows Server AD FS-liittoutumispalvelinten palvelinfarmiin.

3. Määritä Azure Windows Server AD FS-liittoutumispalvelinten välityspalvelimen palvelinklusterin.

Tässä määrityksessä on se etu, vähentää resurssien paikallisen samalla määrittäminen Windows Server AD FS sovelluksia ympäröivän verkossa näyttäminen.

Huomaa, että joko tilanteessa suhteet voidaan muodostaa luottamussuhteet ja lisää tunnistetietojen toimittajat business business yhteiskäytön tarvittaessa.

### <a name="BKMK_CloudSvcConfig"></a>Cloud services-kokoonpanon määritys

Pilvipalveluihin ei tarvita, jos haluat näyttää AM suoraan Internetiin tai näyttää Internetiin yhteydessä oleva kuormituksen sovellus. Tämä on mahdollista, koska kunkin pilvipalvelussa on yksittäinen määritettäviä virtual IP-osoite.

### <a name="BKMK_FedReqVIPDIP"></a>Liitetty viestintä palvelinvaatimukset julkisista ja yksityisistä IP-osoitteet (dynaaminen IP ja virtual IP)

Kunkin Azure virtuaalikoneen vastaanottaa dynaaminen IP-osoite. Dynaaminen IP-osoite on yksityinen osoite, joka on käytettävissä vain Azure sisällä. Useimmissa tapauksissa, se on tarpeen määrittäminen Windows Server AD FS-versioiden virtual IP-osoite. Virtual IP tarvitaan Windows Server AD FS päätepisteet Internet näyttää ja käytetään liitetyt kumppanit ja asiakkaat todennus ja jatkuvaluonteisesta hallinnasta. Näennäinen IP-osoite on ominaisuus, joka sisältää vähintään yhden Azuren näennäiskoneiden pilvipalvelussa. Jos otettu käyttöön Azure- ja Windows Server AD FS saatavat tukeva sovelluksen Internetiin yhteydessä oleva ja jaa yleisiä portit, kunkin edellyttävät omaa virtual IP-osoite ja sen vuoksi on luotava yksi pilvipalvelussa sovelluksen ja Windows Server AD FS sekunneiksi.

Määritelmät virtual IP-osoite ja dynaaminen IP-osoite on artikkelissa [termeistä ja määritelmistä](#BKMK_Glossary).

### <a name="BKMK_ADFSHighAvail"></a>Windows Server AD FS suuren käytettävyyden määrittäminen

Samalla, kun se on mahdollista ottaa käyttöön erillinen Windows Server AD FS-liittoutumispalvelut, on suositeltavaa ottaa käyttöön klusterin vähintään kaksi solmujen kanssa sekä AD FS STS välityspalvelimet tuotannon ympäristössä.

[AD FS 2.0 käyttöönoton topologian huomioon otettavia seikkoja](https://technet.microsoft.com/library/gg982489) -kohdassa [AD FS 2.0-rakenteen opas](https://technet.microsoft.com/library/dd807036) päättää käyttöönoton kokoonpanoasetusten määrittäminen mitä parhaiten vastaa tarpeitasi tietyn.

> [AZURE.NOTE] Jotta saat kuormituksen Azure Windows Server AD FS päätepisteet varten, Määritä Windows Server AD FS-klusterin kaikkien jäsenten saman pilvipalvelussa ja lataa HTTP (oletus 80) Azure Vastatilin ominaisuutta ja HTTPS-portit (oletus 443). Lisätietoja on artikkelissa [Azure kuormituksen näytteenottimen](https://msdn.microsoft.com/library/azure/jj151530).
Windows Server verkon kuormituksen tasaamisen (NLB) ei tueta Azure.
