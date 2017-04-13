<properties
    pageTitle="Liikenteen hallinta päätepisteen tyypit | Microsoft Azure"
    description="Tässä artikkelissa kuvataan erityyppiset päätepisteet, joita voidaan käyttää Azure liikenteen hallinta"
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

# <a name="traffic-manager-endpoints"></a>Päätepisteet liikenteen hallinta

Microsoft Azure liikenteen hallinta voit määrittää, miten verkkoliikennettä jaetaan sovelluksen käyttöönottoa eri palvelinkeskusten käytössä. Voit määrittää kunkin sovelluksen käyttöönottoa nimellä 'päätepisteen' liikenteen hallinta. Kun liikenteen hallinta saa DNS-pyynnön, valitsee käytettävissä päätepisteen DNS vastauksessa. Liikenteen hallinta asettaa valinnan päätepisteen nykyisen tilan ja liikenne reititys-menetelmää. Lisätietoja on artikkelissa [Miten liikenteen hallinta toimii](traffic-manager-how-traffic-manager-works.md).

Tuetut liikenteen hallinta päätepisteen kolme perustyyppiä:

- **Azure päätepisteet** käytetään Azure maksuttomien palveluiden.
- **Ulkoisten päätepisteiden** käytetään palveluiden ulkopuolella Azure, joko paikallisessa tai eri isännöintipalvelussa.
- **Sisäkkäiset päätepisteet** avulla voidaan yhdistää liikenteen hallinta-profiileista luominen joustavammat liikenne reititys-järjestelmiä tukee suurempia, monimutkaisia käyttöönotoissa tarpeisiin.

Ei ole rajoituksen miten päätepisteet erityyppisiä yhdistetään yhteen liikenteen hallinta profiilin. Kunkin profiilin voi olla mikä tahansa valikoima päätepisteen erityyppisiä.

Seuraavissa kohdissa kuvataan kunkin päätepisteen tyyppi yksityiskohtaisemmin.

## <a name="azure-endpoints"></a>Azure päätepisteet

Azure päätepisteet käytetään Azure-pohjaisten palvelujen liikenteen hallinta. Azure resurssityypit tuetaan:

- Perinteinen' IaaS VMs ja PaaS cloud services.
- Web Apps-sovelluksista
- PublicIPAddress resurssit (joka voi yhdistää VMs joko suoraan tai Azure-kuormituksen kautta). PublicIpAddress on oltava DNS myönnetyt voi käyttää liikenteen hallinta profiilin nimi.

PublicIPAddress resurssit ovat Azure Resurssienhallinta. Ne eivät ole perinteinen käyttöönotto-mallissa. Näin ne ovat vain tueta liikenne esimiehen Azure Resurssienhallinta kokemukset. Päätepisteen-tyypit tuetaan Resurssienhallinta ja perinteinen käyttöönoton mallin kautta.

Azure päätepisteet käytettäessä liikenteen hallinta havaitsee, kun 'Perinteinen'-IaaS AM, pilvipalvelussa tai verkkosovellukseen pysäytetään ja käytön aloittaminen. Tämä tila näkyy päätepisteen tila. Katso lisätietoja [liikenteen hallinta päätepisteen seuranta](traffic-manager-monitoring.md#endpoint-and-profile-status) . Kun pohjana palvelu on pysäytetty-liikenteen hallinta Suorita päätepisteen terveystarkastukset tai suora liikenne päätepisteelle. Liikenteen hallinta ei ole laskutuksen tapahtumien pysäytetty esiintymän. Kun palvelu on käynnistettävä uudelleen, laskutuksen ansioluetteloita ja päätepisteen on oikeutettu saamaan liikenne. Tämä tunnistus ei koske PublicIpAddress päätepisteet.

## <a name="external-endpoints"></a>Ulkoisten päätepisteiden

Ulkoisten päätepisteiden käytetään palveluiden Azure ulkopuolella. Esimerkiksi palvelun isännöidään paikallisen tai jokin muu palveluntarjoaja. Ulkoisten päätepisteiden voidaan käyttää yksitellen tai Azure päätepisteet yhdistetty samaan liikenteen hallinta profiilin. Yhdistäminen ulkoisten päätepisteiden Azure päätepisteet mahdollistaa eri tilanteista:

- Joko aktiivinen aktiivinen tai Aktiivinen-passiivinen automaattisesti mallin Käytä Azure parantavat redundancy on aiemmin luodun paikallisen sovelluksen.
- Voit pienentää sovelluksen viive käyttäjille eri puolilla maailmaa laajentaa aiemmin luodun paikallisen sovelluksen muita Maantieteellisten sijaintien Azure-tietokannassa. Lisätietoja on artikkelissa [liikenteen hallinta 'Suorituskyvyn' liikenne reititys](traffic-manager-routing-methods.md#performance-traffic-routing-method).
- Käytä Azure antamaan uusia valmiuksia aiemmin luodun paikalliset sovelluksen jatkuvasti tai "purskeen pilven" ratkaisuksi täyttävän demand Piikkiin.

Joissakin tapauksissa se on hyödyllinen ulkoisten päätepisteiden avulla voit viitata Azure services (esimerkkejä on artikkelissa [usein kysytyt kysymykset](#faq)). Tässä tapauksessa kuntotietojen tarkistus on laskuttaa Azure päätepisteet korko, ei ulkoisten päätepisteiden korko. Kuitenkin toisin kuin Azure päätepisteet Jos lopettaminen tai poistaa pohjana palvelun kunto Tarkista Laskutus jatketaan, kunnes poistaminen käytöstä tai poistaa sen päätepisteestä liikenteen hallinta.

## <a name="nested-endpoints"></a>Sisäkkäiset päätepisteet

Sisäkkäiset päätepisteet yhdistää useita liikenteen hallinta-profiileista, voit luoda joustavia liikenne reititys-järjestelmiä ja tukee suurempia, monimutkaisia käyttöönotoissa tarpeisiin. Sisäkkäiset päätepisteet 'lapsen' profiilin lisätään päätepisteen nimellä "pääkohde-profiiliin. Ali- ja pää-profiileista voi olla mikä tahansa, myös muita sisäkkäisiä profiileja muiden päätepisteet. Lisätietoja on artikkelissa [sisäkkäisiä liikenteen hallinta-profiileista](traffic-manager-nested-profiles.md).

## <a name="web-apps-as-endpoints"></a>Web Apps päätepisteet nimellä

Joitakin muita tapoja käyttää, kun Web Apps-sovellusten määrittäminen kuin päätepisteet liikenteen hallinta:

1. Vain vakio"tuote tai edellä verkkosovelluksissa soveltuvat Käytä liikenteen hallinta. Voit lisätä verkkosovellukseen alemman tuote, yritykset epäonnistuvat. Päivitysprosessin aiemmin Web-sovelluksen SKU hakutulosten liikenteen hallinta lähettäminen enää liikenne kyseisen Web App-sovellukseen.

2. Kun päätepisteen vastaanottaa HTTP-pyynnön, se käyttää pyynnön "isäntä"-otsikko määrittääksesi, mikä Web App-sovelluksen pitäisi pyyntöä. Toimialuenimi on aloittaa pyynnön, kuten contosoapp.azurewebsites.net DNS-nimi. Jos haluat käyttää eri DNS-nimen Web App, DNS-nimi on rekisteröity sovelluksen mukautettua toimialuenimeä. Kun lisäät Web App-päätepisteen kuin Azure päätepisteen, liikenteen hallinta profiilinimi DNS automaattisesti rekisteröity sovelluksen. Tämä rekisteröinti poistetaan automaattisesti, kun päätepisteen poistetaan.

3. Kunkin liikenteen hallinta profiilin voi olla enintään yksi Web App-päätepiste Azure alueittain. Voit kiertää tämän rajoituksen, voit määrittää verkkosovellukseen ulkoinen päätepiste. Lisätietoja on artikkelissa [usein kysytyt kysymykset](#faq).

## <a name="enabling-and-disabling-endpoints"></a>Ottaminen käyttöön ja poistaminen käytöstä päätepisteet

Päätepisteen liikenteen hallinta-toiminnon poistaminen käytöstä voi olla hyötyä liikenne poistaminen väliaikaisesti päätepisteen, joka sijaitsee ylläpidon tila tai on otettava käyttöön uudelleen. Kun päätepisteen on käynnissä uudelleen, voi olla uudelleen käyttöön.

Päätepisteet on käytössä ja käytöstä liikenteen hallinta-portaalissa, PowerShell, CLI tai REST API, mikä tuetaan Resurssienhallinta ja perinteinen käyttöönoton mallin kautta.

>[AZURE.NOTE] Azure päätepisteen poistaminen käytöstä ei mitään tehdään sen käyttöönoton tilan Azure-tietokannassa. Azure palvelu, joka on (kuten AM tai Web Appin pysyy käynnissä ja vastaanottaa liikenne myös silloin, kun käytöstä liikenteen hallinta. Liikenne voidaan korjata suoraan palvelun esiintymän asemesta kautta liikenteen hallinta profiilin DNS-nimi. Lisätietoja on artikkelissa [miten liikenteen hallinta toimii](traffic-manager-how-traffic-manager-works.md).

Päätepisteiden vastaanottamaan tietoliikennettä nykyisen kelpoisuus määräytyy seuraavat seikat:

- Profiilin tila (käytössä/poissa käytöstä)
- Päätepisteen tila (käytössä/poissa käytöstä)
- Kuntotietojen tulokset tarkistaa, että päätepiste

Lisätietoja on artikkelissa [liikenteen hallinta päätepisteen seuranta](traffic-manager-monitoring.md#endpoint-and-profile-status).

>[AZURE.NOTE] Liikenteen hallinta toimii DNS-tasolla, ei voi vaikuttaa minkä tahansa päätepistettä muodostetut yhteydet. Päätepisteen ei ole käytettävissä, kun liikenteen hallinta ohjaa toiseen käytettävissä päätepisteen uusia yhteyksiä. Ei käytössä tai perusasemassa päätepisteen takana host jatkaa kuitenkin vastaanottamaan tietoliikennettä kautta muodostetut yhteydet, kunnes ne istunto lopetetaan. Sovellusten tulisi rajoittaa istunnon kesto sallimaan suodattuu muodostetut yhteydet-liikenne.

Jos kaikki päätepisteet profiilissa on poistettu käytöstä tai itse profiilia ei ole käytettävissä, liikenteen hallinta lähettää 'NXDOMAIN' vastauksen uusi DNS-kysely.

## <a name="faq"></a>USEIN KYSYTYT KYSYMYKSET

### <a name="can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions"></a>Voit käyttää liikenteen hallinta-useita tilauksia päätepisteet?

Käyttämällä päätepisteet-useita tilauksia ei ole mahdollista Azure Web Apps-sovellusten kanssa. Azure verkkosovelluksissa edellyttää, että Web Apps-sovellusten käyttämisestä mukautettua toimialuenimeä käytetään vain yhden tilauksen piiriin kuuluvien. Ei voi käyttää useita tilauksia samassa toimialuenimeä verkkosovelluksissa.

Muuntyyppisten päätepisteen on mahdollista liikenteen hallinta käytettäväksi päätepisteet useita tilauksesta. Miten voit määrittää liikenteen hallinta määräytyy sen mukaan, käytätkö perinteinen käyttöönotto-mallin tai Resurssienhallinta-toiminto.

- Resurssien hallinta päätepisteet minkä tahansa tilauksesi voi lisätä, liikenteen hallinta, kun määrittäminen liikenteen hallinta profiilin henkilön on lukuoikeudet päätepiste. Näiden oikeuksien voi myöntää [Azure Resurssienhallinta Roolipohjainen käyttöoikeuksien valvonta (RBAC)](../active-directory/role-based-access-control-configure.md).
- Perinteinen käyttöönoton malli-käyttöliittymän liikenteen hallinta edellyttää, että pilvipalveluihin tai verkkosovelluksissa määritetty Azure päätepisteet sijaitsevat liikenteen hallinta profiilin saman tilauksen. Muut tilaukset cloud Palvelupäätepisteet voidaan lisätä liikenteen hallinta "ulkoinen" päätepisteet. Nämä ulkoisten päätepisteiden ovat laskuttaa Azure päätepisteet ulkoisen korko sijaan.

### <a name="can-i-use-traffic-manager-with-cloud-service-staging-slots"></a>Voit käyttää liikenteen hallinta pilvipalvelussa väliaikaisen' paikkojen?

Kyllä. Väliaikaisen paikkojen pilvipalvelussa voi määrittää liikenteen hallinta ulkoisten päätepisteiden nimellä. Kuntotietojen tarkistus peritään edelleen Azure päätepisteet korko. Ei ulkoisen päätepisteen tyyppi, koska pohjana palveluun tehtävät muutokset ovat ei noudettu automaattisesti. Ulkoisten päätepisteiden kanssa liikenteen hallinta ei havaitse pilvipalvelussa sijaitsevaan pysäytetty tai poistamisen. Tämän vuoksi liikenteen hallinta jatkuu terveystarkastukset Laskutus, kunnes päätepisteen on poistettu käytöstä tai poistaa.

### <a name="does-traffic-manager-support-ipv6-endpoints"></a>Tukeeko liikenteen hallinta IPv6 päätepisteet?

Liikenteen hallinta ei tarjoa IPv6 addressible nimipalvelimet tällä hetkellä. Liikenteen hallinta voit kuitenkin käyttää edelleen IPv6: n asiakkaiden yhteyden muodostaminen IPv6 päätepisteet. Asiakas ei tehdä DNS-pyyntöihin suoraan liikenne hallintaan. Asiakas käyttää sen sijaan rekursiivinen DNS-palvelusta. Vain IPv6-asiakas lähettää pyynnöt rekursiivinen DNS-palvelun IPv6: N kautta. Valitse rekursiivinen palvelun pitäisi yhteystiedot käyttämällä IPv4 liikenteen hallinta-nimipalvelimet.

Päätepisteen DNS-nimi vastaa liikenteen hallinta. Päätepisteen IPv6-tuki on oltava päätepiste DNS-nimen osoittamalla IPv6 address DNS: n AAAA tietueen. Liikenteen hallinta terveystarkastukset tukevat vain IPv4-osoitteet. Palvelu on samanniminen DNS: n IPv4-päätepisteen näyttää.

### <a name="can-i-use-traffic-manager-with-more-than-one-web-app-in-the-same-region"></a>Voinko käyttää liikenteen hallinta samalla alueella on useita Web Appilla?

Liikenteen hallinta käytetään yleensä-sovellusten käyttöön eri alueilla liikenne voidaan ohjata. Se voidaan myös kohtaa, johon sovellus on useampi kuin yksi käyttöönoton samalla alueella. Liikenteen hallinta Azure päätepisteet eivät salli useamman kuin yhden Web App-päätepisteen saman liikenteen hallinta profiilin lisääminen samaan Azure-alueelta.

Seuraavat vaiheet on vaihtoehtoinen menetelmä voit rajoitusta:

1. Varmista, että päätepisteet ovat eri web app-skaalauksen yksiköt". Toimialuenimi on yhdistettävä yksittäisen sivuston-annetun asteikko. Tämän vuoksi kaksi Web App-sovellusten saman asteikko voi jakaa liikenteen hallinta profiilin.
2. Lisää mukautettu hostname vanity toimialuenimen kunkin Web App-sovellukseen. Kunkin Web-sovelluksen on oltava eri asteikko. Kaikki verkkosovelluksissa on kuuluttava samaan tilaukseen.
3. Lisää yksi (ja vain yksi) Web App-päätepisteen liikenteen hallinta-profiiliisi Azure päätepisteen.
4. Lisää muita Web App-päätepisteiden liikenteen hallinta-profiiliisi ulkoinen päätepiste. Ulkoisten päätepisteiden voidaan lisätä vain käyttämällä resurssien hallinnan käyttöönottomalli.
5. Luo DNS-CNAME-tietue vanity toimialueen, joka osoittaa liikenteen hallinta profiilin DNS-nimi (<>.... trafficmanager.net).
6. Käyttää sivuston kautta vanity toimialuenimi, ei DNS liikenteen hallinta profiilin-nimi.

## <a name="next-steps"></a>Seuraavat vaiheet

- Katso, [miten liikenteen hallinta toimii](traffic-manager-how-traffic-manager-works.md).
- Lisätietoja liikenteen hallinta [päätepisteen seuranta- ja automaattinen automaattisesti](traffic-manager-monitoring.md).
- Lisätietoja liikenteen hallinta [liikenne reititys tavoista](traffic-manager-routing-methods.md).
