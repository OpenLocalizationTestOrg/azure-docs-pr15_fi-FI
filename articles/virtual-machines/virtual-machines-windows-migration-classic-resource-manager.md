<properties
    pageTitle="IaaS resursseja perinteinen Azure resurssien hallinnan ympäristö tuettu siirto | Microsoft Azure"
    description="Tässä artikkelissa käydään läpi ympäristö tuettu siirtämistä resurssit-perinteinen Azure resurssien hallinta"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="kasing"/>

# <a name="platform-supported-migration-of-iaas-resources-from-classic-to-azure-resource-manager"></a>Tuetaan käyttöympäristö siirto IaaS resurssit perinteinen Azure resurssien hallinta

Tässä artikkelissa on kuvaus siitä, miten Microsoft olet käyttöönottoon infrastruktuurin siirto perinteinen-palvelun (IaaS) resursseina Resurssienhallinta käyttöönoton malleihin. Voit lukea lisää tietoja [Azure Resurssienhallinta ominaisuuksiin ja etuihin](../azure-resource-manager/resource-group-overview.md). Kerromme yksityiskohtaisesti yhteyden resursseja, olla tilauksen käyttämällä VPN-sivusto sivusto yhdyskäytävien kaksi käyttöönotto-malleista. 

## <a name="goal-for-migration"></a>Siirron tavoite

Resurssienhallinta käyttöönotto monimutkaisia sovellusten mallien avulla, määrittää näennäiskoneiden käyttämällä AM tunnisteet ja kattaa käyttöoikeushallinta ja tunnisteita. Azure Resurssienhallinta sisältää skaalattava, rinnakkain käyttöönotto näennäiskoneiden käytettävyys joukkoihin. Uusi käyttöönottomalli sisältää elinkaaren hallinta suorittaminen, verkon ja tallennustilaa myös erikseen. Lopuksi ei kohdistus suojauksen ottaminen käyttöön oletusarvoisesti näennäiskoneiden virtual verkoston käyttäminen.

Perinteinen käyttöönoton mallista lähes kaikkia ominaisuuksia tueta suorittaminen, verkon ja tallennustilaa Azure resurssien hallinta. Hyötyvät uusia ominaisuuksia Azure Resurssienhallinta, voit siirtää aiemmin ominaisuuksissa perinteinen käyttöönoton mallista.

## <a name="changes-to-your-automation-and-tooling-after-migration"></a>Automaatio ja sillä siirron jälkeen tehdyt muutokset

Osana resurssien siirtämisestä resurssien hallinnan käyttöönottomalli perinteinen käyttöönotto-malli on päivitettävä aiemmin automaatio tai sillä varmistaa, että se ei toimi siirron jälkeen.

## <a name="meaning-of-migration-of-iaas-resources-from-classic-to-resource-manager"></a>Merkitys IaaS resurssit klassinen resurssien hallinnan siirtäminen

Ennen kuin on siirtyä alaspäin, katsotaan IaaS resurssien tietojen taso ja hallinnan tasolla toimintoja välinen erotus.

- *Hallinnan tasossa* kuvataan puhelut, jotka tulevat hallinta tasossa tai Ohjelmointirajapinnan resurssien muokkaamisen. Esimerkiksi toimintoja, kuten luomisesta AM ja uudelleen AM päivitetään virtual verkon uusi aliverkon hallita käynnissä resursseja. Muutokset eivät vaikuta suoraan yhteyden esiintymät.
- *Tietoja tasossa* (sovelluksen) kuvataan sovelluksesta runtime ja liittyy käsittelemisen esiintymät, jotka eivät siirry Azure-Ohjelmointirajapinnan kautta. Käyttää sivuston tai tietojen suoritettavan SQL Server tai MongoDB palvelimen tietoja pidetään tietojen tasossa tai sovelluksen toimia. Kopioimalla blob storage tilin ja käyttää julkisen IP-osoitteen RDP tai SSH kyselyjä virtuaalikoneen on myös tietojen taso. Näitä toimintoja säilyttää käynnissä suorittaminen, verkko ja tallennustilaa sovelluksen.

>[AZURE.NOTE] Siirron joissakin tilanteissa Azure käyttöympäristö pysähtyy, vapauttaa ja että näennäiskoneiden käynnistyy. Tämä veloitetaan lyhyt tietojen tasossa käyttökatkot.

## <a name="supported-scopes-of-migration"></a>Siirron tuetut alueet

On kolme siirto-alueet, joiden kohteena ensisijaisesti suorittaminen, verkon ja tallennustilaa. 

### <a name="migration-of-virtual-machines-not-in-a-virtual-network"></a>Siirron näennäiskoneiden (eivät sisälly virtual verkon)

Resurssien hallinnan käyttöönotto-mallin suojaus on käytössä sovellustesi oletusarvoisesti. Kaikki VMs on virtual verkostossa Resurssienhallinta-mallissa. Azure ympäristö käynnistyy (`Stop`, `Deallocate`, ja `Start`) VMs siirron osana. Käytettävissä on kaksi vaihtoehtoista virtual verkkojen:

- Voit pyytää uuden virtual verkon luominen ja siirtäminen virtuaalikoneen uusi virtual verkostoon ympäristössä.
- Voit siirtää virtuaalikoneen olemassa olevan virtual verkoston resurssien hallinta.

>[AZURE.NOTE] Siirron tässä alueessa hallinta taso-toimintojen ja tietojen taso-toiminnot voivat ei sallita ajanjakson siirron aikana.

### <a name="migration-of-virtual-machines-in-a-virtual-network"></a>Näennäiskoneiden (virtual verkko)-siirto

Useimmat AM määrityksiä vain metatiedot siirtyminen perinteinen ja resurssien hallinnan käyttöönotto-mallit. Pohjana VMs on käytössä sama laitteisto samassa verkostossa ja saman tallentamiseen. Hallinnan taso-toiminnot voivat ei tietyn ajanjakson aikana siirron aikana. Kuitenkin tietojen tasossa työkirja toimii. Sovellusten käynnissä päälle VMs (perinteinen), maksamaan käyttökatkot ei siirron aikana.

Seuraavat määritykset eivät ole tuettuja. Jos tuki lisätään myöhemmin, jotkin VMs-määritysten voi syntyä käyttökatkot (Siirry – Pysäytä, Poista varaus ja käynnistä se uudelleen AM toimintoja).

-   Sinulla on useampi kuin yksi käytettävyys määrittää yksittäisen pilvipalvelussa.
-   Sinulla on vähintään yksi käytettävyys joukot ja VMs, jotka eivät ole määrittää yksittäisen pilvipalvelussa saatavuus.

>[AZURE.NOTE] Siirron tässä alueessa hallinta tasossa ei voi sallita ajanjakson siirron aikana. Tietyissä käyttömahdollisuudet ohjeiden aiempaa versiota, tiedot tasossa käyttökatkot tapahtuu.

### <a name="storage-accounts-migration"></a>Tallennustilan tilit-siirto

Anna saumaton siirron, voit ottaa Resurssienhallinta VMs perinteinen tallennustilan tilillä. Tätä ominaisuutta Laske että verkkoresursseihin voit ja uuteen ulkopuolisista tallennustilan tilit. Kun siirrät näennäiskoneiden ja VPN, sinun on siirrettävä tallennustilan asiakkaiden suorittamiseen siirtoprosessia päälle. 

>[AZURE.NOTE] Resurssien hallinnan käyttöönottomalli ei ole perinteinen kuvia ja levyjen käsite. Kun tallennustilan tili on siirretty, perinteinen kuvia ja levyjen eivät näy Resurssienhallinta Pinotut mutta varmuuskopioiminen Näennäiskiintolevyjen säilyvät tallennustilan tilin. 

## <a name="unsupported-features-and-configurations"></a>Ei-tuettuja ominaisuuksia ja määritykset

Microsoft ei tällä hetkellä tue joitakin ominaisuuksia ja määrityksiä. Seuraavissa kohdissa kuvataan Microsoftin suositukset niiden ympärille.

### <a name="unsupported-features"></a>Ei-tuettuja ominaisuuksia

Seuraavia ominaisuuksia ei tueta tällä hetkellä. Voit halutessasi poistaa nämä asetukset, siirtää VMs ja ottaa Resurssienhallinta käyttöönoton mallin asetukset uudelleen.

Resurssin tarjoajaan | Toiminto
---------- | ------------
Laske | Liittämättömien virtuaalikoneen levyjä.
Laske | Virtuaalikoneen kuvat.
Verkon | Päätepisteen käyttöoikeusluettelot.
Verkon | VPN yhdyskäytävien (sivuston-sivusto Azure ExpressRoute sovelluksen yhdyskäytävää, osoita sivuston).
Verkon | Virtuaalinen verkot käyttämällä VNet Peering. (VNet siirtäminen ARM sitten vertaiskone) Lisätietoja [VNet Peering] (... /Virtual-Network/Virtual-Network-peering-Overview.MD).
Verkon | Liikenteen hallinta-profiileista.

### <a name="unsupported-configurations"></a>Ei tueta käyttömahdollisuudet

Seuraavat määritykset eivät ole tuettuja.

Palvelun | Määritys | Suositus
---------- | ------------ | ------------
Resurssien hallinta | Roolin perusteella Access ohjausobjektin (RBAC) perinteinen resurssien | Koska resurssit URI on muokattu siirron jälkeen, se kannattaa suunnitella RBAC käytännön päivitykset, jotka on tehtävä, siirron jälkeen.
Laske | Useita aliverkosta AM liittyvät | Päivitä viittaamaan vain aliverkosta aliverkon-määrityksiä.
Laske | Näennäiskoneiden, jotka kuuluvat virtual verkkoon, mutta ei ole eksplisiittinen aliverkon, joka on määritetty | Voit halutessasi poistaa AM.
Laske | Näennäiskoneiden, joiden ilmoitukset-Automaattinen skaalaus käytännöt | Siirtyminen käy läpi ja nämä asetukset olevat kohteet poistetaan. On erittäin suositeltavaa arvioi ympäristösi ennen siirtoa. Voit vaihtoehtoisesti uudelleen ilmoitusasetukset, kun siirto on valmis.
Laske | XML-AM laajennukset (BGInfo 1.*, Visual Studio virheenkorjaus Web ottaminen käyttöön tai Remote virheenkorjaus) | Tämä ei tueta. On suositeltavaa, että poistat laajennusten virtuaalikoneen Jatka siirron tai ne poistetaan automaattisesti siirtoprosessin aikana.
Laske | Käynnistyksen diagnostiikka Premium tallennustilan kanssa | Käytöstä käynnistyksen Diagnostiikka-toiminto VMs ennen jatkamista siirron. Voit ottaa käynnistyksen diagnostiikka Resurssienhallinta Pinotut uudelleen, kun siirto on valmis. Lisäksi BLOB-objektit, jotka ovat käytössä näyttökuva ja serial lokit on poistettava niin näitä BLOB enää perittävän.
Laske | Cloud Services-palvelut, jotka sisältävät web/työntekijä roolit | Tämä on tällä hetkellä tueta.
Verkon | Virtuaalinen verkkoja, jotka sisältävät näennäiskoneiden ja web/työntekijä roolit |  Tämä on tällä hetkellä tueta.
Azure sovelluksen-palvelu | Virtuaalinen verkkoja, jotka sisältävät sovelluksen palvelun ympäristöissä | Tämä on tällä hetkellä tueta.
Azure Hdinsightiin | Virtuaalinen verkkoja, jotka sisältävät HDInsight-palvelut | Tämä on tällä hetkellä tueta.
Microsoft Dynamics elinkaari-palvelut | Virtuaalinen verkkoja, jotka sisältävät näennäiskoneiden, joita hallitaan Dynamics elinkaari-palvelut | Tämä on tällä hetkellä tueta.
Laske | Azure Tietoturvakeskus laajennusten VNET, jossa on VPN-yhdyskäytävän tai Kannattaa yhdyskäytävän paikallinen DNS-palvelimeen | Azure Tietoturvakeskus asentaa laajennukset automaattisesti oman näennäiskoneiden niiden suojaus ja nosta ilmoitukset. Näitä tunnisteita yleensä asennetaan automaattisesti Jos Azure Tietoturvakeskus-käytäntö otetaan käyttöön tilauksen. Yhdyskäytävän siirto ei tueta tällä hetkellä ja yhdyskäytävä on poistettava ennen jatkamista vahvistetaan siirron, internet-yhteyttä AM-tallennustilan tilin menetetään, kun yhdyskäytävän poistetaan. Siirto ei voi jatkaa, kun näin tapahtuu, kun Vieras agentti tila Blob-objektien ei voi täyttää. On suositeltavaa poistaa käytöstä Azure Tietoturvakeskus käytännön tilauksen 3 tuntia, ennen kuin jatkat siirron.

## <a name="the-migration-experience"></a>Siirto-toiminto

Ennen kuin aloitat siirron kokemus, suositellaan seuraavasti:

- Varmistaa, että resurssit, jotka haluat siirtää ei käyttää minkä tahansa ominaisuuksia tai määrityksiä. Yleensä platform tunnistaa ongelmat ja aiheuttaa virheen.
- Jos sinulla on VMs, jotka eivät ole virtual verkossa, ne pysäytetään ja Varaus poistettu valmistelu osana. Jos et halua menettää julkinen IP-osoite, tarkista kyselyjä varaaminen ennen käynnistävä valmistelu IP-osoite. Jos VMs ovat virtual verkkoon, ne ovat ei pysäytetty ja Varaus poistettu.
- Suunnittele Valmistele aikana ei-aukioloajat, jotta saat odottamattomia virheitä, joka voi käydä siirron aikana.
- Lataa oman VMs nykyiset määritykset käyttämällä helpompi kelpoisuuden valmisteleminen vaiheen jälkeen PowerShell, käyttöliittymä (CLI) komennot tai REST API.
- Päivitä automaatio/operationalization komentosarjojen käsittelemään resurssien hallinnan käyttöönottomalli, ennen kuin aloitat siirron. Voit halutessasi tehdä GET-toimintoja, kun resurssit ovat valmiita-tilaan.
- Arvioi RBAC käytännöt, jotka on määritetty perinteinen IaaS resursseja ja kun siirto on valmis suunnitteleminen.

Siirtämisen työnkulku on seuraava

![Näyttökuva, jossa näkyy siirtämisen työnkulku](./media/virtual-machines-windows-migration-classic-resource-manager/migration-workflow.png)

>[AZURE.NOTE] Kaikki toiminnot, joka on kuvattu seuraavissa osissa on idempotent. Jos sinulla on ongelmia kuin ominaisuus jota ei tueta tai määritysvirhe, on suositeltavaa valmisteleminen, yritä uudelleen Keskeytä tai Vahvista toiminto. Azure-ympäristö pyritään toiminnon uudelleen.

### <a name="validate"></a>Vahvista

Vahvista-toiminto on siirtoprosessia ensimmäisessä vaiheessa. Tämä vaihe on palauttaa onnistumista/virheen, jos resurssit ovat voi siirron ja analysoida tietoja, valitse siirron resurssien taustalla.

Valitset virtual verkon tai isännöityä palvelun (jos se ei ole virtual verkon), johon haluat vahvistaa siirtoa varten.

* Jos resurssi ei voi siirron, Azure-ympäristö on lueteltu kaikki syitä, miksi sitä ei tueta siirron.

### <a name="prepare"></a>Valmisteleminen

Valmistele-toiminto on siirtoprosessia toinen vaihe. Tässä vaiheessa lähinnä simuloida IaaS resurssit perinteinen Resurssienhallinta resurssien muunnos ja esittää tämä rinnakkain, jossa voit havainnollistaa.

Valitset virtual verkon tai isännöityä palvelun (jos se ei ole virtual verkon), johon haluat siirtoon valmistautuminen.

* Jos resurssi ei voi siirron, Azure käyttöympäristö lopettaa siirtoprosessia ja syyn, miksi valmistelu epäonnistui.
* Jos resurssin siirron, Azure ympäristö ensimmäisen lukituksia alas kohdassa siirron resurssien hallinta taso-toimintoja. Esimerkiksi et pysty lisäämään tietoja DVD-levyllä AM siirto-kohdassa.

Azure-ympäristö sitten alkaa metatietojen siirron klassinen resurssien hallinnan siirtäminen resurssien.

Kun valmistelu on valmis, voit halutessasi havainnollistetaan usein sekä klassinen resurssien ja Resurssienhallinta. Perinteinen käyttöönoton mallin jokaisen pilvipalvelussa, Azure ympäristö Luo on resurssin nimen, joka on kuvion `cloud-service-name>-migrated`.

>[AZURE.NOTE] Näennäiskoneiden, joita ei ole perinteinen Virtual Network pysäytetään deallocated siirron tässä vaiheessa.

### <a name="check-manual-or-scripted"></a>Tarkista (manuaalinen tai komentosarjaan määritetty)

Tarkistus-vaiheessa voit käyttää myös määrityksistä, jotka olet ladannut aiemmin Vahvista siirron näkyy oikein. Vaihtoehtoisesti voit voit kirjautua portaalin ja paikalla tehtävien ominaisuudet ja vahvista, että metatietojen siirron näyttää hyvältä resurssit.

Jos olet siirtymässä virtual verkkoon, ei ole useimmat määrittäminen näennäiskoneiden käynnistetään uudelleen. Sovellusten näiden VMs voit vahvistaa, että sovellus on edelleen hyvin alkuun.

Voit testata seuranta/automaatio ja toiminnallisia komentosarjat VMs toimivat odotetusti, ja jos päivitetty komentosarjat toimivat oikein. Vain GET-toiminnot ovat tuettuja, kun resurssit ovat valmiita-tilaan.

Ei ole määrittäminen aikaikkunan, jota ennen haluat Vahvista siirron. Voi kestää mahdollisimman paljon aikaa kuin haluat tässä tilassa. Kuitenkin hallinta tasossa on lukittu näiden resurssien, ennen kuin voit keskeyttää tai Vahvista.

Jos huomaat ongelmia, voit aina keskeyttää siirron ja palaa perinteiseen käyttöönottomalli. Kun palaat takaisin, Azure ympäristö Avaa resurssien hallinnan taso-toimintojen niin, että voit palauttaa ne VMs perinteinen käyttöönoton mallin Normaali toimenpiteet.

### <a name="abort"></a>Keskeytys

Keskeytys on valinnainen vaihe, joiden avulla voit palauttaa perinteinen käyttöönoton mallin muutokset ja Pysäytä siirto.

>[AZURE.NOTE] Tätä toimintoa ei voi suorittaa, kun olet käynnistänyt vahvistaminen.  

### <a name="commit"></a>Vahvista

Kun vahvistus on valmis, voit vahvistaa siirto. Resurssit eivät näy enää perinteinen ja ovat käytettävissä vain resurssien hallinnan käyttöönottomalli. Siirretyt resursseja voidaan hallita vain uuden portaalin.

>[AZURE.NOTE] Tämä on idempotent-toimintoa. Jos se epäonnistuu, on suositeltavaa yritä uudelleen. Jos se säilyy epäonnistuu, tuki lippu tai luoda keskustelupalstan viestin ClassicIaaSMigration-tunnisteella sekä [AM keskustelupalstalle](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows).

## <a name="frequently-asked-questions"></a>Usein kysytyt kysymykset

**Koskeeko tämä siirron suunnitelma Omat olemassa olevia palveluja tai sovelluksia, jotka toimivat Azuren näennäiskoneiden?**

Ei. (Perinteinen) VMs ovat täysin tuettu palvelujen yleiseen käyttöön. Voit edelleen näiden resurssien avulla voit laajentaa oman vaatiman Microsoft Azure-tallennustilan.

**Mitä tapahtuu Omat VMs, jos voin enkä aio siirtämisestä lähitulevaisuudessa?**

Olemme ovat ei kuinka aiemmin perinteinen ohjelmointirajapinnan ja resurssin malli. Haluamme helpottavat siirron lähitulevaisuudessa lisäominaisuuksia, jotka ovat käytettävissä resurssien hallinnan käyttöönottomalli. On erittäin suositeltavaa, että luet [osa Lisää](virtual-machines-windows-compare-deployment-models.md) , jotka ovat osa IaaS Valitse Resurssienhallinta.

**Miten tämä siirron suunnitelma vaikuttaa Omat aiemmin sillä?**

Päivittää oman sillä resurssien hallinnan käyttöönottomalli on yksi tärkeimmät muutokset, jotka ovat laskemisesta siirron suunnitelmien.

**Kuinka kauan hallinta tasossa käyttökatkot on?**

Odotusaika riippuu on siirrettävä resurssien määrän. Pienempi käyttöönotoissa (VMs muutaman kymmenlukuun) koko siirron olisi otettava alle tunnissa. Siirron voi kestää muutaman tunnin suurissa käyttöönotoissa (VMs satoja).

**Voin siirtää takaisin, kun siirretään resurssien on vahvistettu resurssien hallinta?**

Valmistele voi keskeyttää, koska resurssit ovat valmiita-tilaan. Peruuttaminen ei tueta, kun resursseja on siirretty onnistuneesti vahvistaminen kautta.

**Voit voin aikaisempi Omat siirron Jos vahvistus epäonnistuu?**

Siirto ei voi keskeyttää, jos vahvistus epäonnistuu. Kaikki siirtotoiminnot, mukaan lukien Vahvista-toiminto on idempotent. Suosittelemme näin suoritettava toiminto lyhyen ajan kuluttua uudelleen. Jos edelleen tuloste virheen, luoda tuki lippu tai keskustelupalstan viestin sekä [AM keskustelupalstalla](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows)ClassicIaaSMigration-tunnisteella.

**Onko minun ostaa toisen express reitin piiri, jos minun käyttää IaaS Valitse Resurssienhallinta?**

Ei. Olemme äskettäin käyttöön [siirtäminen ExpressRoute piirit-perinteinen Resurssienhallinta käyttöönotto-malliin](../expressroute/expressroute-move.md). Sinun ei tarvitse ostaa uusi ExpressRoute piiri, jos sinulla on jo jokin.

**Entä jos minulla oli määritetty perinteinen IaaS resurssien Roolipohjainen käyttöoikeuksien valvonta käytäntöjä?**

Siirron aikana resurssit transform-perinteinen resurssien hallinta. Niin kannattaa suunnitella RBAC käytännön päivitykset, jotka on tehtävä, siirron jälkeen.

**Entä jos käytän Azure palauttaminen tai Azure varmuuskopioinnin tänään?**

Voit siirtää virtuaalikoneen, joissa on otettu käyttöön varmuuskopion, katso [voin varmuuskopioinut Omat perinteinen VMs varmuuskopion säilöön. Nyt haluan siirtää Omat VMs perinteistä Resurssienhallinta-tilassa. Miten voin varmuuskopioida ne palautus services säilöön?](../backup/backup-azure-backup-ibiza-faq.md#i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-to-migrate-my-vms-from-classic-mode-to-resource-manager-mode-how-can-i-backup-them-in-recovery-services-vault)

**Voin tarkistaa tilauksen- tai resurssien nähdäksesi, jos ne ovat voi siirron?**

Kyllä. Ympäristö tuettu siirto-asetus siirron valmistautumisessa ensimmäiseksi on Vahvista, että resurssit pystyvät siirron. Siltä varalta, kun validate epäonnistuu, vastaanotat viestejä kaikki siirto ei voi viimeistellä syistä.

**Mitä tapahtuu, jos voin tulla kiintiön Virhe valmisteltaessa IaaS resurssit siirtoa varten?**

Suosittelemme, että Keskeytä Valmistele ja kirjaudu sitten niin, että alueessa, jos olet siirtymässä VMs kiintiön tukipyynnön. Kun kiintiön pyyntö hyväksytään, voit aloittaa siirron vaiheiden suorittamista uudelleen.

**Miten voin raportoida ongelma?**

Kirjaa ongelmat ja siirron koskeviin kysymyksiin sekä [AM keskustelupalstalla](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows)avainsanan ClassicIaaSMigration kanssa. On suositeltavaa kirjauksen kaikkiin kysymyksiisi tämän keskustelupalstalle. Jos sinulla on tuen sopimus, olet Tervetuloa kirjautua tuki-lippu.

**Entä jos voin et pidä siitä, jotka platform ohittanut siirron aikana resurssien nimien?**

Kaikki resurssit, jotka sisältävät nimiä perinteinen käyttöönoton mallin erikseen säilyvät siirron aikana. Joissakin tapauksissa luodaan uusia resursseja. Esimerkki: verkkoliittymän luodaan jokaisen AM. Voit hallita näitä siirron aikana luodaan uusi resurssien nimet ei tällä hetkellä tueta. Kirjaudu oman äänet tämän ominaisuuden käyttämiseen [Azure palautetta keskustelupalstalle](http://feedback.azure.com).

* *sain viestin *"AM raportoi yleinen agent-tila ei ole valmiina. Näin ollen AM ei voi siirtää. Varmista, että AM-agentti raportoi yleisen agentti tilan valmiiksi"* tai *"AM sisältää tunniste, jonka tila raportoidaan ei-AM. Näin ollen tämän AM ei siirretä."***

Tämä tulee sanoma, kun AM ei saa sisältää lähteviä internet-yhteys. AM-agentti käyttää lähtevien yhteyksien saavuttamiseksi Azure-tallennustilan tilin agent-tilan päivittäminen viiden minuutin välein.


## <a name="next-steps"></a>Seuraavat vaiheet
Nyt kun tiedät, että perinteinen IaaS resurssien resurssien hallinnan siirron, voit aloittaa siirtyminen resurssit.

- [Tekniset perinpohjaisesti käsittelevään artikkeliin-ympäristö tuettu siirron-perinteinen Azure resurssien hallinta](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
- [Siirtää IaaS resurssien perinteinen Azure resurssien hallinta PowerShellin avulla](virtual-machines-windows-ps-migration-classic-resource-manager.md)
- [CLI avulla voit siirtää IaaS resurssien perinteinen Azure resurssien hallinta](virtual-machines-linux-cli-migration-classic-resource-manager.md)
- [Kloonaa käyttämällä yhteisön PowerShell-komentosarjojen perinteinen virtual machine Azure resurssien hallinta](virtual-machines-windows-migration-scripts.md)
