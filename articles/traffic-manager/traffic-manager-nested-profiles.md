<properties
    pageTitle="Ylemmän tason liikenteen hallinta-profiileista | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan ylemmän tason profiilit-ominaisuus Azure liikenteen hallinta"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="nested-traffic-manager-profiles"></a>Sisäkkäiset liikenteen hallinta-profiileista

Liikenteen hallinta sisältää liikenteen reititys tavoista, jotta voit määrittää, miten liikenteen hallinta valitsee mitä päätepisteen vastaanottavien liikenne kunkin peruskäyttäjän alueen. Lisätietoja on artikkelissa [liikenteen hallinta liikenne reititys tavoista](traffic-manager-routing-methods.md).

Kunkin liikenteen hallinta profiilin määrittää yksittäisen liikenne reititys-menetelmällä. On kuitenkin skenaariot, jotka tarvitsevat kehittyneempiä liikenne reititys kuin reititys myöntämä yhden liikenteen hallinta profiilin. Voit sisällyttää liikenteen hallinta-profiileista, jos haluat yhdistää useamman kuin yhden liikenne reititys menetelmä edut. Sisäkkäiset profiilien avulla voit ohittaa liikenteen hallinta oletustoiminnan suurempi tuki-ja monimutkaisia sovelluksen käyttöönottoa.

Seuraavissa esimerkeissä kuvataan sisäkkäisiä liikenteen hallinta-profiileista käyttämisestä eri tilanteista.

## <a name="example-1-combining-performance-and-weighted-traffic-routing"></a>Esimerkki 1: Yhdistäminen 'Suorituskyky' ja 'Painotettu' liikenne reititys

Oletetaan, että käyttöön sovelluksen Azure seuraavilla alueilla: Länsi US Länsi Europe ja Itä-Aasian. Liikenteen hallinta 'Suorituskyvyn' liikenne reititys menetelmän avulla voit jakaa-liikenne paikalliseen lähimpänä olevan käyttäjälle.

![Yhden liikenteen hallinta-profiili][1]

Oletetaan, haluat testata palvelun päivitys, ennen kuin juoksevan Lisää laajasti. Haluat ohjata pienelle liikenteen testi käyttöönottoon painotettu' liikenne reititys menetelmän avulla. Voit määrittää testin käyttöönotto aiemmin tuotannon käyttöönoton rinnalla Länsi Euroopan.

Et voi yhdistää molempien 'painotettu' ja ' suorituskyvyn liikenne-reitityksestä yhtä profiilia. Tässä skenaariossa tukemaan luot liikenteen hallinta profiilin Länsi Europe päätepisteet ja 'Painotettu' liikenne reititys-menetelmää. Seuraavaksi lisäät 'lapsen' profiilia päätepisteen 'ylätason'-profiiliin. Ylemmän tason profiilin edelleen käyttää suorituskyvyn liikenne reititys-menetelmää, ja se sisältää muita kuin päätepisteet yleinen ominaisuuksissa.

Seuraavassa kaaviossa on kuvattu tässä esimerkissä:

![Sisäkkäiset liikenteen hallinta-profiileista][2]

Tässä määrityksessä liikenne ohjataan kautta ylemmän tason profiilin jakaa liikenne alueiden normaalisti. Länsi Euroopan sisäkkäisiä profiilin jakaa liikenne tuotannon ja testaa päätepisteet määritetty leveydet mukaan.

Kun ylemmän tason profiilin käyttää 'Suorituskyvyn' liikenne reititys-menetelmää, kukin päätepiste on määritettävä sijainti. Sijainti määritetään, kun määrität päätepiste. Valitse käyttöönoton lähinnä Azure alue. Azure alueiden on tuettu Internet viive-taulukko sijainti-arvoa. Lisätietoja on artikkelissa [liikenteen hallinta 'Suorituskyvyn' liikenne reititys menetelmä](traffic-manager-routing-methods.md#performance-traffic-routing-method).

## <a name="example-2-endpoint-monitoring-in-nested-profiles"></a>Esimerkki 2: Päätepisteen seuranta ylemmän tason-profiileista

Liikenteen hallinta valvoo aktiivisesti päätepisteiden palvelun kuntoa. Jos päätepisteen perusasemassa, liikenteen hallinta ohjaa käyttäjät vaihtoehtoinen päätepisteet säilyttää palvelun käytettävyyttä. Tämä päätepiste seuranta- ja automaattisesti ongelma koskee kaikkia liikenne reititys menetelmiä. Lisätietoja on artikkelissa [Liikenteen hallinta päätepisteen seuranta](traffic-manager-monitoring.md). Sisäkkäiset profiilit päätepisteen seuranta toimii eri tavalla. Sisäkkäiset profiileja ylemmän tason profiilin ei suorita kuntotietojen tarkistus-lapsen suoraan. Sen sijaan lapsen profiilin päätepisteet kunto voidaan laskea lapsen profiilin yleinen kunto. Kuntotietojen tiedot on välitetty sisäkkäisiä profiilin hierarkian ylöspäin. Ylemmän tason profiilin tämä koostetun kunto voit selvittää, suora liikenne ali-profiiliin. Artikkelissa on tämän artikkelin tiedot [usein kysytyt kysymykset](#faq) -osassa kunnon valvonta sisäkkäisiä profiilit.

Palaaminen edellisessä esimerkissä oletetaan tuotannon käyttöönotto Länsi Euroopan epäonnistuu. Oletusarvon mukaan 'lapsen' profiilin ohjaa kaikki liikenne testi käyttöönottoon. Jos testi käyttöönoton myös epäonnistuu, ylemmän tason profiilin määrittää, että lapsen profiilin olisi vastaanota liikenne koska kaikki lapsen päätepisteet perusasemassa. Sitten ylätason profiilin jakaa muilla alueilla liikennettä.

![Sisäkkäiset profiilin automaattisesti (oletusasetus)][3]

Haluat ehkä Tässä vaihtoehdossa tyytyväinen. Tai haluat ehkä kyseisen, että kaikki liikenne Länsi Eurooppa nyt siirtymällä testi käyttöönoton sijaan rajoitettu osajoukko-liikenne. Testi-käyttöönoton kunto riippumatta haluat epäonnistua päälle muihin alueille, kun tuotannon käyttöönotto Länsi Euroopan epäonnistuu. Tämä automaattisesti käyttöön voit määrittää 'MinChildEndpoints'-parametrin määritettäessä lapsen profiilin kuin päätepisteen pää-profiiliin. Parametrin määrittää käytettävissä päätepisteet lapsen profiilin vähimmäismäärä. Oletusarvo on '1'. Tässä skenaariossa MinChildEndpoints-arvo arvoksi 2. Tämä raja-arvo alla ylemmän tason profiilin katsoo ei ole käytettävissä koko ali-profiilin ja ohjaa liikenne muiden päätepisteet.

Seuraavassa kuvassa on esimerkki määritysten:

![Ylemmän tason profiilin automaattisesti kanssa 'MinChildEndpoints' = 2][4]

>[AZURE.NOTE]
>"Prioriteetti-liikenne reititys tapa jakaa kaikki liikenne paikalliseen yhden loppupisteen. Näin on pieni tarkoitus MinChildEndpoints asetusta kuin '1' lapsen profiili.

## <a name="example-3-prioritized-failover-regions-in-performance-traffic-routing"></a>Esimerkki 3: Ensin automaattisesti alueiden 'Suorituskyvyn' liikenne reititys

Menetelmä 'Suorituskyvyn' liikenne reititys oletusominaisuuksien on suunniteltu ladataan perusteettomasti lähimpään päätepisteen seuraava ja aiheuttaa virheitä CSS sarjan välttämiseksi. Kun päätepisteen epäonnistuu, kaikki liikenne, jotka on ohjattu kyseisen päätepisteen jaetaan tasaisesti muiden päätepisteet kaikkien alueiden välillä.

!['Suorituskyvyn' liikenne reititys oletusarvon automaattisesti kanssa][5]

Oletetaan kuitenkin, että mieluummin Länsi Europe liikenne vikasietotilaa Länsi US ja muiden alueiden liikenteen suoraan vain, kun molemmat päätepisteet eivät ole käytettävissä. Voit luoda tämän ratkaisun käyttämällä lapsen profiilia "prioriteetti-liikenne reititys-menetelmällä.

!['Suorituskyvyn' liikenne reititys etuuskohteluun automaattisesti kanssa][6]

Koska Länsi Europe päätepisteen on suurempi kuin Länsi US päätepisteen prioriteetti, kaikki liikenne lähetetään Länsi Europe päätepisteen, kun molemmat päätepisteet ovat online-tilassa. Jos Länsi Europe epäonnistuu, sen liikenne ohjataan Länsi US. Sisäkkäiset profiiliin liikenne ohjataan Itä-Aasian vain silloin, kun Länsi Europe ja Länsi US epäonnistuu.

Kaikkien alueiden Toista tätä mallia. Korvaa kaikki kolme päätepisteet ylemmän tason profiilin kolme ali-profiileista kunkin antamisen Priorisoidut automaattisesti järjestyksessä.

## <a name="example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region"></a>Esimerkki 4: Hallinta 'Suorituskyvyn' liikenne reititys useita päätepisteet saman alueen välillä

Oletetaan, että "suorituskyvyn' liikenne reititys menetelmä käytetään profiilia, joka on useampi kuin yksi päätepiste tietyn alueen. Oletusarvon mukaan ohjataan alueen liikenne jaetaan tasaisesti kaikki käytettävissä olevat päätepisteet kyseisen alueen yli.

!['Suorituskyvyn' liikenne reititys-alueen liikenne jakauma (oletusasetus)][7]

Sen sijaan, että lisäät useita päätepisteet Länsi Euroopan, kyseiset päätepisteet alussa ja lopussa on erillinen lapsen profiilin. Lapsen profiilin lisätään ylemmän tason Länsi Euroopan vain päätepiste. Lapsen profiilin asetuksia voit määrittää liikenne-jakauma ja jonka Länsi Europe ottamalla prioriteetti-pohjainen tai painotetut liikenne reititys alueella olevat.

![Mukautettu alue-liikenne jakaumaan reititys 'Suorituskyvyn' liikenne][8]

## <a name="example-5-per-endpoint-monitoring-settings"></a>Esimerkki 5: Päätepisteen kohti valvonta-asetukset

Oletetaan, että käytät liikenteen hallinta siirtää sujuvasti vanha tietoliikenteen paikalliset uusi pilvipohjainen versio ylläpidettävä Azure sivusto. Aiempien versioiden sivuston, jota haluat käyttää kotisivun URI kuntotietojen seurannassa. Mutta uusi pilvipohjainen versioksi käyttöönoton mukautettu seuranta-sivu (polku "/ monitor.aspx"), joka sisältää muita tarkistukset.

![Liikenteen hallinta päätepisteen seuranta (oletusasetus)][9]

Liikenteen hallinta profiilin valvonta-asetukset soveltuvat kaikki päätepisteet sisällä yhden profiilin. Sisäkkäiset profiileja avulla eri lapsen profiili-sivustoa kohden Määritä eri valvonta-asetukset.

![Liikenteen hallinta päätepisteen seurantaa päätepisteen asetukset][10]

## <a name="faq"></a>USEIN KYSYTYT KYSYMYKSET

### <a name="how-do-i-configure-nested-profiles"></a>Miten sisäkkäisiä profiilit määritetään?

Sisäkkäiset liikenteen hallinta-profiileista voi määrittää Azure Resurssienhallinta ja perinteinen Azure REST API-Azure PowerShell cmdlet-komentojen ja Office kaikissa ympäristöissä Azure CLI komentoja. Ne tuetaan myös uuden Azure portaalin kautta. Hän ei tue perinteinen-portaalissa.

### <a name="how-many-layers-of-nesting-does-traffic-manger-support"></a>Kuinka monta kerrokset upottamalla käytettäessä liikenteen hallinta tukee?

Voit sisällyttää profiilit enintään 10 syvyys tasoa. Silmukat' eivät ole sallittuja.

### <a name="can-i-mix-other-endpoint-types-with-nested-child-profiles-in-the-same-traffic-manager-profile"></a>Muuntyyppisten päätepisteen sekoita sisäkkäisiä ali-profiileista samaan liikenteen hallinta profiilin kanssa

Kyllä. Ei ole mitään rajoituksia, miten voit yhdistää päätepisteet erityyppisiä profiilin kuluessa.

### <a name="how-does-the-billing-model-apply-for-nested-profiles"></a>Miten Laskutus mallin käyttöön sisäkkäiset profiilien?

Ei ole negatiivinen hinnoittelua vaikutus sisäkkäisiä profiilien käyttäminen.

Liikenteen hallinta laskutus on kaksi komponenttia: päätepisteen terveystarkastukset ja miljoonia DNS-kyselyt

- Päätepisteen kuntotietojen tarkistus: on maksutonta lapsen profiili, kun päätepisteen pää-profiiliin määritetty. Lapsen profiilin päätepisteet seuranta on laskuttaa tavalliseen tapaan.
- DNS-kyselyissä: kummankin kyselyn mukaan ainoastaan kerran. Ylemmän tason profiilia, joka palauttaa päätepisteen lapsen profiilista kysely lasketaan vain ylemmän tason profiilin vastaan.

Katso tarkat tiedot [hinnoittelu liikenteen hallinta-sivu](https://azure.microsoft.com/pricing/details/traffic-manager/).

### <a name="is-there-a-performance-impact-for-nested-profiles"></a>Onko sisäkkäisiä profiilien vaikutus suorituskykyyn?

Ei. Ei suorituskyvyn vaikutusta kertyneet sisäkkäisiä profiilit käytettäessä.

Liikenteen hallinta-nimipalvelimet siirtää profiilin hierarkian sisäisesti käsiteltäessä kukin DNS-kysely. DNS-kyselyn ylemmän tason profiiliin saavat DNS-vastauksen kanssa päätepisteen lapsen profiilista. Yksittäisen CNAME-tietue käytetään, onko käytössäsi on yhden profiilin tai sisäkkäisiä profiileja. Ei ei tarvitse luoda CNAME-tietue kunkin profiilin hierarkiassa.

### <a name="how-does-traffic-manager-compute-the-health-of-a-nested-endpoint-in-a-parent-profile"></a>Miten liikenteen hallinta Laske sisäkkäisiä päätepisteen pää-profiilin kunto?

Ylemmän tason profiilin ei tehdä kuntotietojen tarkistus-lapsen suoraan. Sen sijaan lapsen profiilin päätepisteet kunto käytetään lapsen profiilin yleinen kunto laskemiseen. Nämä tiedot on välitetty sisäkkäisiä profiilin hierarkian sisäkkäisiä päätepisteen terveydentilasta ylöspäin. Ylemmän tason profiilin käyttää tätä koostetun kunto määrittääksesi, onko liikenne ohjataan lapsen.

Seuraavassa taulukossa on kuvattu toimintaa, liikenteen hallinta kunto tarkistaa sisäkkäisiä päätepisteelle.

|Lapsen profiilin näytön tila|Ylemmän tason päätepisteen näytön tila|Huomautuksia|
|---|---|---|
|Ei käytössä. Lapsen profiilin on poistettu käytöstä.|Pysäytetty|Ylemmän tason päätepisteen tila on pysäytetty ole poistettu käytöstä. Ei käytössä-tilaan on varattu, joka ilmaisee, että olet poistanut päätepisteen pää-profiiliin.|
|Heikentynyt. Vähintään yksi lapsen profiilin päätepiste on heikentynyt-tilaan.| Online: Online päätepisteet lapsen profiilin määrä on vähintään MinChildEndpoints arvo.<BR>CheckingEndpoint: Online- plus CheckingEndpoint päätepisteet lapsen profiilin määrä on vähintään MinChildEndpoints arvo.<BR>Heikentynyt: muulla tavoin.|Liikenne reititetään päätepisteen tilan CheckingEndpoint. Jos MinChildEndpoints on liian suuri, sen päätepisteestä on aina heikentynyt.|
|Online-tilassa. Vähintään yksi lapsen profiilin päätepiste on Online-tilaan. Päätepistettä on heikentynyt-tilassa.|Katso.||
|CheckingEndpoints. Vähintään yksi lapsen profiilin päätepiste on "CheckingEndpoint". Päätepisteitä ovat "Online" tai "Heikentynyt"|Sama kuin edellä.||
|Passiivinen. Kaikki lapsen profiilin päätepisteet on poistettu käytöstä tai pysäytetty tai tämän profiilin on päätepisteitä.|Pysäytetty||


## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [liikenteen hallinta toiminta](traffic-manager-how-traffic-manager-works.md)

Lisätietoja [liikenteen hallinta-profiilin](traffic-manager-manage-profiles.md) luominen

<!--Image references-->
[1]: ./media/traffic-manager-nested-profiles/figure-1.png
[2]: ./media/traffic-manager-nested-profiles/figure-2.png
[3]: ./media/traffic-manager-nested-profiles/figure-3.png
[4]: ./media/traffic-manager-nested-profiles/figure-4.png
[5]: ./media/traffic-manager-nested-profiles/figure-5.png
[6]: ./media/traffic-manager-nested-profiles/figure-6.png
[7]: ./media/traffic-manager-nested-profiles/figure-7.png
[8]: ./media/traffic-manager-nested-profiles/figure-8.png
[9]: ./media/traffic-manager-nested-profiles/figure-9.png
[10]: ./media/traffic-manager-nested-profiles/figure-10.png

