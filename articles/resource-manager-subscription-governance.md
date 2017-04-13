<properties
   pageTitle="Parhaat käytännöt Azure siirtäminen yrityksille | Microsoft Azure"
   description="Tässä artikkelissa kuvataan, yrityksille avulla voit varmistaa ympäristössä, jossa on suojattu ja helpommin hallittaviin scaffold."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="rdendtler"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="rodend;karlku;tomfitz"/>

# <a name="azure-enterprise-scaffold---prescriptive-subscription-governance"></a>Azure yrityksen scaffold - ominaisuuksien tilauksen hallinnon

Yrityksille yhä standardeja julkisen Pilvipalvelun sen joustavuutta ja joustavasti. He ovat käyttävien Luo tuotto tai optimointi resurssien liiketoiminnan vahvuuksien ja jakaminen pilvessä. Microsoft Azure on, että yritysten voit koota rakenneosien sähköpostiosoite laajan toiminnoista ja sovellukset, kuten monipuolisia palveluita. 

Mutta tietää, missä voit aloittaa kangertelee usein – oli. Sopiva käyttämään Azure joitakin kysymyksiä syntyä yleisesti:

- "Kuinka voin vaatimusten Microsoftin oikeudelliset tiedot paikallisen tietosuojan joissakin maissa?"
- "Miten voin varmistaa, että joku ei ole vahingossa muuttanut kriittinen järjestelmän?"
- "Mistä tietää, mitä resurssi on tukevat niin voin tili, johon se ja se takaisin tarkasti laskun?"

Tyhjä tilauksen, jossa ei ole suojakaiteet mahdollinen asiakas on kovin. Tätä tyhjää tilaa voi ilmetä ongelmia Azure siirtäminen.

Tässä artikkelissa tekninen ammattilaisille hallinnon tarpeesta ja saldo joustavuutta tarvetta pohjana. Se esittelee enterprise-scaffold, joka ohjaa toteuttaminen ja niiden Azure tilausten hallinta organisaatioiden käsite. 

## <a name="need-for-governance"></a>Hallinnon tarve

Kun Siirry Azure ja tulee ratkaista hallinnon etuajassa yrityksen sisällä pilveen onnistuu käytön varmistamiseksi aihe. Valitettavasti aika- ja bureaucracy luominen täydellinen hallinnon järjestelmän tarkoittaa liiketoiminnan joitakin ryhmiä siirtyä suoraan toimittajille eivät yrityksen IT. Tätä tapaa kannattaa pitää yrityksen auki heikkouksien, jos resurssit hallitaan ei ole oikein. Julkisen cloud - joustavuutta, joustavuutta ja kulutus perustuva hinnoittelu - ominaisuudet ovat tärkeitä liiketoiminta-ryhmiin, jotka on nopeasti vastaavat asiakkaiden (sisäisten ja ulkoisten). Mutta yrityksen IT on varmistettava suojattu tehokkaasti tiedot ja kaaviota.

Todellinen elinkaaren rakennustelineet käytetään luomaan perustana olevan rakenne. Scaffold ohjaa yleinen rakenne ja tarjoaa ankkurin kohtien järjestelmään pysyvämpään Systemsin. Yrityksen scaffold on sama: joukko joustavia ohjausobjekteja ja Azure rakennettu julkisen cloud Services-ympäristön ja ankkurit tyyleillä ominaisuuksia. Se sisältää Builder (IT ja business ryhmät) luominen ja liitä uusien palvelujen foundation.

Scaffold perustuu emme ole kerättyjen monta työt-asiakasohjelmien kanssa erikokoisia käytännöt. Näiden asiakkaiden vaihtelevat yksinkertaisista Fortune 500 yritykset ja riippumattomat toimittajat, jotka ovat siirtäminen ja pilveen, ratkaisujen kehittämiseen pilveen, ratkaisujen kehittämiseen pienissä organisaatioissa. Yrityksen scaffold on "purpose-built" joustava tukemaan perinteinen IT-toiminnoista ja joustava työmääriä; kuten kehittäjien ohjelmiston nimellä--palvelun (SaaS) sovellusten perusteella Azure ominaisuuksia.

Yrityksen scaffold on tarkoitettu minkä kunkin uuden tilauksen Azure kuluessa. Sen avulla järjestelmänvalvojat voivat varmistaa työmääriä täyttävät vähintään hallinnon organisaation ilman estää business-ryhmät ja kehittäjät kokouksen nopeasti omia tavoitteet.

> [AZURE.IMPORTANT] Hallinnon on Azure onnistumista. Tässä artikkelissa kohteena yrityksen scaffold tekninen soveltaminen, mutta vain koskettaa laajempi prosessin ja osien väliset suhteet. Käytännön jatkuu ylhäältä alas ja määräytyy liiketoiminnan haluaa saavuttamiseksi. Tietenkin Azure hallintomallin luonnin sisältää edustajia IT, mutta tärkeintä tunnisteen pitäisi olla vahva edustuksen liiketoiminnan ryhmän täytemerkkejä ja tietoturvan ja risk hallinta. Lopuksi yrityksen scaffold on tietoja pienentävät liiketoiminnan riskejä helpottamiseksi organisaation toiminta ja tavoitteet.

Seuraavassa kuvassa kuvataan scaffold osat. Perusta on riippuvainen tasainen suunnitteleminen Osastot, asiakkaat ja tilaukset. Pylväiden koostuvat Resurssienhallinta käytännöt ja vahva nimeämiskäytännön mukaisia. Loput scaffold tulee core Azure ominaisuuksia ja ominaisuuksiin, ota käyttöön ympäristössä, jossa on suojattu ja helpommin hallittaviin.

![scaffold osat](./media/resource-manager-subscription-governance/components.png)

> [AZURE.NOTE] Azure on kasvanut nopeasti, koska sen johdanto 2008: ssa. Tämä kasvu tarvitaan Microsoft suunnittelu ryhmiä tietoturvaan liittyvien ongelmien niiden lähestymistapa hallinta ja käyttöönotto palvelut. Azure Resurssienhallinta-mallin otettiin käyttöön 2014 ja korvaa perinteinen käyttöönottomalli. Resurssien hallinnan avulla organisaatiot voivat helpottaa käyttöön, järjestäminen ja hallita Azure resurssit. Resurssienhallinta sisältää parallelization luotaessa resurssit monimutkaisia, sarjaksi ratkaisuja nopeampaa käyttöönottoa varten. Se myös hajautetun käyttöoikeuksien hallinta ja mahdollisuuden pitää metatietojen tunnisteen resursseilla. Microsoft suosittelee, että luot kaikkien resurssien Resurssienhallinta-mallin avulla. Yrityksen scaffold on suunniteltu erikseen Resurssienhallinta-malli.

## <a name="define-your-hierarchy"></a>Määritä hierarkian

Minkä scaffold ovat Azure yrityksen rekisteröinti (ja yritysportaalin). Yrityksen rekisteröinti määrittää muodon ja käyttää Azure palvelujen yrityksen sisällä ja on core hallinnon rakenne. Enterprise Agreement-sopimus asiakkaat ovat voi jakaa edelleen ympäristön Osastot, asiakkaat ja lopuksi tilaukset. Azure tilaus on kohtaa, johon sisältyvät kaikki resurssit. Määrittää on myös useita rajojen sisällä Azure, kuten sydämiä, resursseista jne.

![hierarkia](./media/resource-manager-subscription-governance/agreement.png)

Jokaisen yrityksen on eri ja edellisen kuvan hierarkian mahdollistaa merkittäviä joustavuutta näin Azure on järjestetty yrityksen sisällä. Ennen toteuttaminen tämän asiakirjan ohjeet mallin hierarkian ja ymmärtää Laskutus, resurssien ja monimutkaisuus.

Kolme yleisiä kuvioiden määrittäminen Azure rekisteröintejä ovat seuraavat:

- **Toimi** kuvio

    ![toimi](./media/resource-manager-subscription-governance/functional.png)

- **Liiketoiminnan yksikkö** -kaava 

    ![Business](./media/resource-manager-subscription-governance/business.png)

- **Maantieteelliset** kuvio

    ![maantieteelliset](./media/resource-manager-subscription-governance/geographic.png)

Voit käyttää scaffold Tilauksen tasolla Laajenna hallinnointitapa vaatimukset yrityksen tilaukseen.

## <a name="naming-standards"></a>Standardit nimeäminen

Scaffold ensimmäisen palvelinvirtualisointi on nimeäminen mukaisia. Tyylikkäitä nimeämiskäytäntö standardeja avulla voit tunnistaa resurssit-portaalissa, laskun ja komentosarjoja. Todennäköisesti sinulla on jo nimeäminen standardeja paikallinen infrastruktuurin. Kun lisäät Azure-ympäristöön, voit laajentaa nimeämiskäytäntö standardit Azure resurssien. Nimeäminen standard helpottamiseksi tehokkaampaa hallinta ympäristön kaikille tasoille.

> [AZURE.TIP]
>
> - Tarkista ja antaa where mahdollista [kuvioiden ja käytännöt ohjeet](./guidance/guidance-naming-conventions.md). Näiden ohjeiden avulla voit päättää kuvaava nimeämiskäytäntö vakio.
> - Käytä camelCasing (kuten myResourceGroup ja vnetNetworkName) resurssien nimet. Huomautus: On tiettyjä resursseja, kuten tallennustilan tilit, missä ainoa tapa käyttää pienet kirjaimet (ja muita erikoismerkkejä).
> - Kannattaa käyttää Azure Resurssienhallinta käytännöt (on kuvattu seuraavassa osassa) voidaan valvoa nimeämiskäytännön mukaisia.
 
## <a name="policies-and-auditing"></a>Käytännöt ja valvonta

Scaffold toisen palvelinvirtualisointi liittyy luominen [Azure Resurssienhallinta käytännöt](resource-manager-policy.md) ja [valvonta tehtävän loki](resource-group-audit.md). Resurssienhallinta käytännöt antaa sinulle mahdollisuuden Azure riskien hallinta. Voit määrittää käytäntöjä, jotka varmistavat tietojen paikallisen tietosuojan rajoittaminen, pakotetut tai tiettyjen toimintojen tarkistaminen. 

- Käytäntö on oletusarvoinen **Salli** järjestelmä. Voit hallita toiminnot määrittämällä ja käytäntöjen määrittäminen resursseja, Estä tai valvonta toiminnot resursseja.
- Käytännöt on kuvattu käytäntöjen määritelmät käytännön määritys kielellä (Jos sitten edellytykset) mukaan.
- Voit luoda käytäntöjä JSON (Javascript Object Notation) kanssa muotoillut tiedostot. Käytännön määritettyäsi voit määrittää sen mahdollistavat tietyn laajuiset: tilaus, resurssiryhmä. tai resurssi.

Käytännöt on useita toimintoja, jotka sallivat tarkasti rajattuja lähestymistapa tekemät. Toiminnot ovat:

-   **Estä**: estää resurssin pyyntö
-   **Valvonta**: sallii pyynnön, mutta lisää rivin tehtävän lokiin (joka voidaan säätää ilmoituksia tai käynnistettävän runbooks)
-   **Liitä**: Lisää resurssin määritetyt tiedot. Jos resurssille ei ole "CostCenter" tunnisteen, Lisää oletusarvoista sama tunniste.

### <a name="common-uses-of-resource-manager-policies"></a>Resurssienhallinta käytännöt Yleiset käyttötavat

Azure Resurssienhallinta käytännöt on tehokas työkalu Azure työkalujen. Niiden avulla voit välttää odottamattomat kustannukset, voit tarkistaa resurssien – tunnisteita kustannuspaikka ja varmistaa, että compliancy vaatimukset täyttyvät. Kun käytännöt yhdistetään valmiin valvontatoimintoja, voit fashion monimutkaisia ja joustavia ratkaisuja. Käytännöt Salli yritysten ohjausobjektit työmääriä "Perinteinen IT" ja "Agile" työmääriä; kuten kehityssovellusten asiakkaalle. Yleisimmät kuviot näemme koskevat ovat seuraavat:

- **Geo-yhteensopivuustiedot paikallisen tietosuojan** - Azure on alueiden kaikkialla maailmassa. Yritykset haluavat usein määrittää, missä resurssit luodaan (vai varmistaa tietojen paikallisen tietosuojan vain Varmista resurssien luodaan lähellä resurssit Lopeta kuluttajille).
- **Kustannusten hallinnan** - Azure tilauksen voi sisältää resurssien monta ja asteikko. Yritykset haluavat usein varmistaa, että vakio tilaukset Vältä tarpeettoman resurssit, jotka voit maksaa satoja dollareina kuukausi tai Lisää.
- **Oletus hallinnon kautta tarvittavat tunnisteet** - edellyttävän tunnisteet on yleisin ja erittäin haluamasi ominaisuudet. Azure Resurssienhallinta käytäntöjen avulla yritysten ovat voit varmistaa, että resurssi on merkitty asianmukaisesti. Yleisimmät tunnisteet ovat: osasto, resurssien omistajan ja ympäristön tyyppi (esimerkiksi – tuotannon, Testaa, kehittäminen)

**Esimerkkejä**

"Perinteinen IT-tilauksen liiketoiminta-sovellukset

-   Pakota osasto- ja omistajuustiedot tunnisteita voi tarkastella kaikkien resurssien
-   Rajoittaa resurssin luomista Pohjois-Amerikka-alue
-   Mahdollisuus luoda G sarjan VMs ja HDInsight klustereiden rajoittaminen

"Joustava" ympäristön business yksikön cloud sovellusten luominen

- Tietoja paikallisen tietosuojan vaatimusten Salli resurssien luonti vain tietyllä alueella.
- Viite-ympäristön kaikki resurssit-tunniste. Jos resurssi on luotu ilman tunnistetta, Liitä **ympäristössä: Tuntematon** resurssin tunnisteen.
- Valvonta, kun resursseja luodaan Pohjois-Amerikan ulkopuolella, mutta ei estä.
- Valvonta, kun suuri kustannusresurssit luodaan.

> [AZURE.TIP]
>
> Resurssienhallinta käytännöt organisaatioissa yleisin käyttö on ohjausobjektiin, *jossa* resursseja voi luoda ja *resurssityyppejä* voi luoda. Lisäksi antamisen ohjausobjekteissa *mihin* ja *miten*monta yrityksille käytät käytännöt resursseja on tarvittavat metatiedot laskutusosoite takaisin kulutus. On suositeltavaa käytäntöjen tasolla tilauksen ottamisesta käyttöön:
>
> - GEO-yhteensopivuustiedot paikallisen tietosuojan
> - Kustannusten hallinta
> - Pakollinen tunnisteet (Determined tarpeellinen, kuten BillTo-sovelluksen omistajan mukaan)
>
> Voit käyttää alemman tason vaikutusalueen muita käytännöt.
 
### <a name="audit---what-happened"></a>Valvonta - mitä on tapahtunut?

Voit tarkastella ympäristön toimintaa, haluat käyttäjän valvoa. Useimmat resurssityypit sisällä Azure Luo vianmäärityslokit, voit analysoida log-työkalun tai Azure toimintojen hallinta ohjelmalla tehdystä tiedostosta. Voit kerätä lokeja useiden tilaukset antamaan osaston tai enterprise-näkymä. Valvontatiedot ovat tärkeitä diagnostiikkatyökalu ja tärkeä, jolla käynnistimen tapahtumien Azure-ympäristössä.

Lokeja resurssin Managerista ominaisuuksissa avulla voit määrittää **toimintoja** , joita noudatit Aseta ja suorittanut ne. Lokeja voi kerätä ja koostetun työkaluilla, kuten Log Analytics.

## <a name="resource-tags"></a>Resurssin tunnisteet

Kun organisaation käyttäjien resurssien lisääminen tilaukseen, tulee tärkeämpää resurssien liittäminen asianmukainen, asiakkaan ja ympäristössä. Voit liittää metatietojen resursseja [tunnisteet](resource-group-using-tags.md)kautta. Tunnisteiden avulla on tietoja resurssi tai omistaja. Tunnisteiden avulla voit koota ei vain ja ryhmitellä resurssit eri tavalla, mutta tiedot palautus tarkoituksiin. Voit lisätä tunnisteen resursseja korkeintaan 15: arvo-pareina. 

Resurssin tunnisteet ovat joustavia ja liitetään useimmat resurssit. Esimerkkejä yleisten resurssien tunnisteet ovat:

- BillTo
- Osasto (tai Business yksikkö)
- Ympäristön (tuotannon-vaiheessa kehitys)
- Taso (Web taso, sovelluksen taso)
- Sovelluksen omistaja
- Projektin nimi

![tunnisteet](./media/resource-manager-subscription-governance/resource-group-tagging.png)

Katso Lisää esimerkkejä tunnisteet [nimeämiskäytännöt Azure resurssien suositus](./guidance/guidance-naming-conventions.md).

> [AZURE.TIP]
>
> Luo tunnisteiden strategia, joka määrittää oman tilauksissa mitä metatietojen tarvitaan business, talous, suojaus, riskinhallinta ja ympäristön Yleinen hallinta. Harkitse tekemistä käytännön, joka valtuuttaa tunnisteita varten:
>
> - Resurssiryhmät
> - Tallennustilan
> - Näennäiskoneiden
> - Sovelluksen palvelun ympäristöissä/web-palvelimet

## <a name="resource-group"></a>Resurssiryhmä 

Resurssien hallinnan avulla voit sijoittaa resurssien hallinta-laskutustietojen tai luonnollinen affiniteetti kuvaava-ryhmiin. Kuten edellä mainittiin, Azure on kaksi käyttöönoton mallit. Aiemmissa perinteinen-malli basic mittayksikön hallinta on tilaus. On vaikea eritellä tilauksen, joka johtanut paljon tilaukset luominen resursseja. Resurssienhallinta-mallissa on tuli resurssiryhmät esittely. Resurssiryhmät ovat resursseja on yleisiä elinkaari tai jakaa määritteen, kuten "SQL-palvelimilta" tai "A".

Resurssiryhmiä et voi sisältyä sisäkkäistä ja resursseja voi kuulua vain yksi resurssiryhmä. Voit käyttää kaikkia resursseja resurssiryhmän tiettyjen toimintojen. Esimerkiksi resurssiryhmä poistaminen poistaa kaikki resursseja resurssiryhmän kuluessa. Yleensä sijoitettu koko sovelluksen tai siihen liittyvä saman resurssiryhmä. Esimerkiksi Contoso-Web-sovelluksen-nimisen kolme taso-sovelluksen sisältäisi verkkopalvelin, sovelluspalvelin ja SQL server saman resurssiryhmä.

> [AZURE.TIP]
>
> Miten resurssiryhmien järjestää voi vaihdella "Perinteinen IT" työmääriä "joustava IT-toiminnoista
>
> - "Perinteinen IT-toiminnoista on ryhmitelty yleisimmin saman elinkaari, kuten sovelluksen kohteita. Ryhmittely-sovelluksen avulla yksittäiset sovellusten hallinta.
> - "Joustava IT" työmääriä yleensä keskittyminen ulkoisen asiakkaan osoittava cloud sovellukset. Resurssiryhmät on vastattava (esimerkiksi Web taso-sovelluksen taso) käyttöönoton kerrokset ja hallinta.

## <a name="role-based-access-control"></a>Roolipohjainen käyttöoikeuksien valvonta

Todennäköisesti pyydät itse "kuka pitäisi olla resurssien käytön?" ja "miten hallinta käyttöoikeuden" Sallimisen tai hylätä access Azure-portaaliin ja resurssit-portaalin käytön hallinnasta on tärkeää. 

Kun Azure alun perin julkaistiin, access-ohjausobjektit tilaukseen on basic: tai muiden järjestelmänvalvojat. Käyttää perinteinen mallin implisiittinen Accessin portaalissa kaikille resursseille tilaukseen. Tämä puuttuminen tarkasti rajattuja ohjausobjektin johtanut tilauksia koskevia säätää Azure-rekisteröinti suositeltavaa kerralla käytönvalvonta taso.

Tämä tilauksia koskevia ei enää tarvita. Roolipohjainen käyttöoikeuksien valvonta, jossa voit määrittää käyttäjät vakio rooleille (esimerkiksi "" ja "" tavallisia roolit). Voit myös määrittää mukautettuja rooleja.

> [AZURE.TIP]
>  
> - Yhdistä yrityksen käyttäjätiedot tallennetaan (yleisimmin Active Directory) Azure Active Directory AD Connect-työkalun avulla.
> - Ohjausobjekti-järjestelmänvalvoja ja Mää-järjestelmänvalvojan käyttämällä Hallittavan jäsenyyden tilauksen. Järjestelmänvalvoja ja Mää-järjestelmänvalvoja **ei** Määritä uusi tilauksen omistaja. Käytä RBAC roolit antamaan **omistajan** oikeudet ryhmälle tai yksittäisen.
> - Azure käyttäjien lisääminen ryhmään (esimerkiksi sovelluksen X omistajat) Active Directoryn. Synkronoidun ryhmän avulla voit antaa ryhmän jäsenten hallinta resurssiryhmä, joka sisältää sovelluksen tarvittavia oikeuksia.
> - Noudata myöntämisestä Odotettu työn tekemiseen tarvittava **käyttöoikeuksien myöntäminen** tarpeen. Esimerkki:
>     - Käyttöönotto-ryhmä: Ryhmä, johon kuuluu vain käyttöönotto resurssit.
>     - Virtuaalikoneen hallinta: Ryhmä, joka pystyy käynnistäminen uudelleen VMs (toiminnot)

## <a name="azure-resource-locks"></a>Azure resurssien lukitukset

Kun organisaation Lisää core services tilaukseen, on tärkeämpää varmistaa, että nämä palvelut ovat käytettävissä business keskeytymisen. [Resurssien lukitukset](resource-group-lock-resources.md) avulla voit rajoittaa toimintoja suuria resursseja, jossa Muokkaa tai poista niitä on merkittävä vaikutus sovellusten tai pilvi-infrastruktuuria. Voit käyttää lukitukset tilauksen, resurssiryhmä tai resurssi. Yleensä haluat käyttää lukitukset foundational resurssit, kuten virtual verkkojen, yhdyskäytävien ja tallennustilaa tilit. 

Resurssien lukitukset tällä hetkellä tueta kaksi arvoa: CanNotDelete ja vain luku-tilassa. CanNotDelete tarkoittaa, että käyttäjät (jossa on tarvittavat käyttöoikeudet) näkyvät edelleen lukea tai muokata resurssin, mutta sitä ei voi poistaa. Vain luku tarkoittaa, että valtuutettujen käyttäjien ei voi poistaa tai muokata resurssin.

Voit luoda tai poistaa lukitusten hallinta, on pääsy `Microsoft.Authorization/*` tai `Microsoft.Authorization/locks/*` toiminnot.
Valmiiden roolien omistaja ja käyttäjän käyttöoikeus järjestelmänvalvoja myönnetään nämä toiminnot.

> [AZURE.TIP] Core verkkoasetukset suojattava lukitukset. Yhdyskäytävän vahingossa-sivusto sivusto VPN on suuria Azure-tilaukseen. Azure ei salli Poista virtuaalikoneen verkossa, jossa on käytössä, mutta otetaan käyttöön lisärajoituksia on hyötyä peruslevyä. Käytännöt ovat myös tärkeitä hallintatoiminnot ylläpitoon. On suositeltavaa, että asennat **CanNotDelete** lukitusta käytännöt, jotka ovat käytössä.
>
> - VPN: CanNotDelete
> - Verkko-käyttöoikeusryhmän: CanNotDelete
> - Käytännöt: CanNotDelete

## <a name="core-networking-resources"></a>Core verkko-resurssit

Resurssien käytön voi olla sisäisen (sisällä yrityksen verkkoon) tai ulkoisen (Internetistä). On helppo sijoittaa vahingossa väärään kohtaan resurssit ja avata ne mahdollisesti haitallisen käytön organisaation käyttäjien. Paikallinen-laitteiden kanssa yrityksille on lisättävä hallintatoiminnot varmistaa, että Azure käyttäjät tekevät oikeita päätöksiä. Tarkastellaan tilauksen hallinnon core resursseja, joissa on basic käytön valvonta. Core resurssit koostuvat:

- **Virtual** verkkoja aliverkosta säilön objektit. Vaikka ei ole välttämätöntä, sitä käytetään usein sisäinen yritysresurssit sovellusten muodostettaessa.
- **Verkon käyttöoikeusryhmät** muistuttavat palomuuri ja anna sääntöjä, miten resurssin "puhua" verkon kautta. Hajautetun päättää, miten niissä / Jos aliverkon (tai virtuaalikoneen) muodostaa yhteyden Internetiin tai muita aliverkosta virtual samassa verkostossa.

![Verkko](./media/resource-manager-subscription-governance/core-network.png)

> [AZURE.TIP]
>  
> - Luo virtual verkkojen ulkoisessa toiminnoista ja sisäisessä toiminnoista. Tätä tapaa vähentää asettamisesta vahingossa näennäiskoneiden, jotka on tarkoitettu sisäisten työmääriä ulkoisen aukeaman tilaan.
> - Verkon SharePoint-ryhmät ovat tärkeitä määritysten. Vähintään käytön estäminen Internet sisäinen virtual verkoista ja yrityksen verkkoon access estää ulkoiset verkostot virtual.

### <a name="automation"></a>Automaatio

Resurssien hallinta yksitellen on molemmat aikaa ja mahdollisesti virhe voi enää tiettyjä toimintoja varten. Azure on eri automaatio-ominaisuuksia, kuten Azure automaatio, logiikan sovellukset ja Azure-funktiot. [Azure automaatio](./automation/automation-intro.md) avulla järjestelmänvalvojat voivat luoda ja määrittää runbooks käsittelee Yleiset toiminnot resurssien hallinta. Voit luoda runbooks PowerShell-koodi-editori tai graafisen editorin avulla. Voi tuottaa monitasoisia monivaiheinen työnkulkuja. Azure automaatio käytetään yleensä käsittelemään tavallisia tehtäviä, kuten sulkemista käyttämättömät resurssit ja resurssien luominen tiettyyn käynnistimen vastauksen tarvitsematta ihmisen.

> [AZURE.TIP]
>
> - Luo Azure automaatio-tili ja tarkista käytettävissä olevat runbooks (graafisen sekä komennon rivi) käytettävissä [Runbookin valikoima](./automation/automation-runbook-gallery.md).
> - Tuo ja mukauttaa avaimen runbooks omaa käyttöäsi varten.
>
> Käytetty vaihtoehto on Käynnistä/Sammuta näennäiskoneiden aikataulun mahdollisuus. On esimerkki runbooks valikoimassa käytettävissä olevat, sekä käsitellä tässä skenaariossa opeta kuinka sen laajentamiseksi.


## <a name="azure-security-center"></a>Azure Tietoturvakeskus

Esimerkiksi jokin suurin sallittu cloud käyttöönoton on ollut huolenaiheet suojauksen päälle. IT riskin valvojat ja suojauksen osastot on varmistaa, että Azure resurssit ovat suojattuja. 

[Azure Tietoturvakeskus](./security-center/security-center-intro.md) tarjoaa keskitetyn näkymän tilausten resurssien suojauksen tilan ja neuvoo, joka estää vaarantuneen resursseja. Voit ottaa käyttöön eritellympiä käytäntöjä (esimerkiksi kohdistaa käytännöt tietyn resurssin ryhmiin, joka mahdollistaa yrityksen sovittaa niiden reagoimisessa ne täyttävät riskiin). -Azure Tietoturvakeskuksessa on avoinna ympäristö, jonka avulla Microsoft-kumppaneille ja riippumattomat toimittajat voivat luoda ohjelmia, joka kytketään Azure Tietoturvakeskus parantaa sen ominaisuuksia. 

> [AZURE.TIP]
>
> Azure Tietoturvakeskuksessa on oletusarvoisesti jokaisen tilauksen. Sinun on otettava näennäiskoneiden sallimaan Azure Tietoturvakeskus Aloita tietojen kerääminen ja asentaa sen agentti tietojen kokoelmasta.
>
> ![tietojen kerääminen](./media/resource-manager-subscription-governance/data-collection.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- Nyt kun olet sai tilauksen hallinnon, on aika Nähdäksesi suositukset käytännössä. Esimerkkejä [käyttöönoton Azure tilauksen hallinnon](resource-manager-subscription-examples.md).

*Ohjeaihe on lähettänyt [Karl Kuhnhausen](https://github.com/karlkuhnhausen) .*
