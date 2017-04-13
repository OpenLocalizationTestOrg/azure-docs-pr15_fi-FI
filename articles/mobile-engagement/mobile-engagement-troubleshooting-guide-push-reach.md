<properties 
   pageTitle="Azure Vianmääritysoppaan - Push/Reach Mobile välitys" 
   description="Käyttäjän vuorovaikutus ja ilmoituksen Azure Mobile välitys ongelmien vianmääritys" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="troubleshooting-guide-for-push-and-reach-issues"></a>Vianmääritys oppaan Push ja saavuttaa ongelmat

Seuraavassa on ja miten Azure Mobile välitys lähettää tietoja käyttäjien viestejä mahdolliset ongelmat.
 
## <a name="push-failures"></a>Push-virheet

### <a name="issue"></a>Ongelma
- Työntää eivät toimi (Valitse app-sovelluksen tai molemmat).

### <a name="causes"></a>Syitä
- Monta kertaa push virhe on maininta, että Azure Mobile välitys, Reach tai toisen lisäominaisuudet ja Azure Mobile välitys ei ole oikein integroitu tai, että päivitys vaaditaan SDK: ssa voit korjata tunnettuja ongelmia uusi OS tai laitteen ympäristö.
- Testaa vain sovellusten push ja vain määrittääksesi, jos järjestelmässä on sovellusten tai poissa-sovelluksessa ongelma poissa-sovelluksen push.
- Testaa Käyttöliittymä ja Ohjelmointirajapinnan ongelmanratkaisuvaiheeksi virheen tiedoista on käytettävissä molemmista sijainneista.
- Ulos sovelluksesta työntää eivät toimi, ellei Azure Mobile välitys ja Reach integroidaan SDK: ssa.
- Työntää ei toimi, jos varmenteet eivät ole kelvollisia, tai käytät tuot ja keskihajonta oikein (vain iOS). (**Huomautus:** "ulos sovelluksesta" push-ilmoitukset saa toimittaa iOS-laitteille, jos molemmat kehittäminen (keskihajonta) ja tuotannon (tuot) versioita asennettuna samaan laitteeseen, koska suojauksen salasana liittyvät varmennetta sovelluksen voidaan mitätöidään Apple mukaan. Voit ratkaista ongelman, poista sovelluksen keskihajonta ja tuot versiot ja asenna uudelleen vain yksi versio laitteessasi.)
- Sovelluksen push ulos laskee käsitellään eri tavalla eri ympäristöissä (iOS näkyy vähemmän kuin Android tietoja, jos laitteessa on poistettu käytöstä alkuperäisen työntää, Ohjelmointirajapinnan voit antaa lisätietoja kuin Käyttöliittymän push tilasto).
- Ulos sovelluksesta työntää voidaan estää asiakkaiden OS tasolla (Live v2.0.).
- Ulos sovelluksesta työntää näytetään, kun käytöstä Azure Mobile välitys-käyttöliittymä, jos ne eivät ole yhdistetty oikein, mutta voi epäonnistua äänettömästi Ohjelmointirajapinnan.
- Sovelluksen työntää eivät toimi, ellei Azure Mobile välitys ja Reach integroidaan SDK: ssa.
- GCM ja ADM työntää eivät toimi, ellei Azure Mobile välitys ja palvelin on integroitu SDK (vain Android).
- Sovelluksen ja sovelluksen poissa työntää on testattava erikseen, onko Push tai Reach ongelma.
- Työntää edellyttää, että sovellus Avaa Vastaanotettava-sovelluksessa.
- Sovelluksen työntää ovat usein asennusohjelma voi suodattaa suostumuksen antamista tai hakuja tiedot hyperlinkkiosoitteen.
- Jos käytät mukautetun luokan Reach-sovellusten ilmoitusten, toimi koskeva ilmoitus oikea elinkaaren, tai muuten ilmoituksen ei voi tyhjentää kun käyttäjän hylätä sen.
- Jos markkinointikampanjan aloittaminen päättymispäivä ja laite vastaanottaa tekstimuodossa sovelluksen ilmoituksen mutta ei ole sitä vielä, käyttäjän on edelleen saa ilmoituksen seuraavan kerran kirjautuvat sovellukseen, vaikka lopettaa markkinointikampanja manuaalisesti näytön.
- Push-Ohjelmointirajapinnan ongelmien Varmista, että todella haluat käyttää Push-Ohjelmointirajapinnan sijaan saavuttaa Ohjelmointirajapinnan (koska Ohjelmointirajapinnan yhteys käytetään usein) ja että "tiedot" ja "ilmoitus"-parametrit on helpon ei.
- Testaa push-markkinointikampanjan sekä WIFI ja 3G taustaäänten mahdollista lähteenä ongelmia verkkoyhteyden kautta liitetyn laitteen kanssa.

## <a name="push-testing"></a>Push-testaaminen

### <a name="issue"></a>Ongelma
- Työntää voidaan lähettää tietyn laitteen mukaan laitteen tunnus.

### <a name="causes"></a>Syitä

- Testaa laitteet ovat asennus eri ympäristöissä eri tavalla, mutta aiheuttavat tapahtuman sovelluksen testi laitteessa ja etsit laitteen ID-tunnuksena portaalissa pitäisi toimia Etsi laitteen tunnus kaikissa ympäristöissä.
- Testaa laitteet toimivat eri tavalla IDFA ja IDFV (vain iOS).


## <a name="push-customization"></a>Push-mukauttaminen

### <a name="issue"></a>Ongelma
- Laajennettu push sisällön kohde ei toimi (henkilökortin soi värinätoiminnot, kuva, jne.).
- Työntää linkit eivät toimi (ulos sovelluksesta sovelluksessa linkeiltä, sovelluksen kohtaan).
- Push-tilastotiedot Näytä, että push ei lähetetty mahdollisimman paljon vastaanottajia oikein (liian paljon tai liian vähän).
- Push kaksoiskappale ja vastaanotettujen kahdesti.
- Ei voi rekisteröidä testilaitteen varten Azure Mobile välitys Vie (oman tuot tai keskihajonta-sovellus).

### <a name="causes"></a>Syitä

- Linkki tiettyyn kohtaan sovelluksessa edellyttää "luokat" (vain Android).
- Laaja linkittäminen kalastelulta ohjaamaan käyttäjät, kun olet napsauttanut push-ilmoituksen vaihtoehtoiseen sijaintiin on luotu ja hallitsee sovelluksesi ja laitteen OS Mobile välitys mukaan suoraan. (**Huomautus:** ulos sovelluksesta ilmoituksia ei voi linkittää suoraan app sijainneissa, jossa on iOS kuin Androidin kanssa.)
- Ulkoinen kuva palvelinten täytyy käyttää HTTP "GET" ja "HEAD" varten ylemmän tason Vie toimimaan (vain Android).
- Omassa koodissasi käytöstä Azure Mobile välitys-agentti näppäimistön avattaessa ja on koodisi Azure Mobile välitys-agentti aktivoida uudelleen, kun näppäimistön on suljettu, niin, että näppäimistö ei vaikuta ilmoitus (vain iOS) ulkoasua.
- Jotkin kohteet eivät toimi testi simulaatioita, mutta vain reaali Kampanjat (henkilökortin soi värinätoiminnot, kuva, jne.).
- Ei ole palvelimeen puolen tiedot kirjataan, kun "testata" työntää-painikkeen avulla. Tiedot kirjataan vain reaali push kampanjat.
- Auttamaan eristää ongelmaa vianmääritys: Testaa, simuloida, ja reaali markkinointikampanjan, koska ne kaikki toimivat hieman eri tavalla.
- Ajanjakso, että "sovelluksessa" ja "aina" Kampanjat ajoitetut voi vaikuttaa toimituksen numeroita, koska markkinointikampanjan toimitetaan vain käyttäjät, jotka ovat "sovelluksessa" markkinointikampanja suorittamisen aikana (ja käyttäjät, joilla on niiden laiteasetusten asettaminen ilmoituksia "ulos sovelluksesta").
- Miten Android- ja iOS käsittelee ulos sovellusten ilmoitukset erot vaikeuttaa verrata suoraan push tilastotiedot sovelluksen Android- ja iOS-version välillä. Android sisältää enemmän OS tason ilmoituksen tietoa kuin iOS. Android-raporttien alkuperäisen ilmoituksen on vastaanotettu, napsautettaessa tai poistaa ilmoituksen keskelle, mutta iOS raportoi tiedot, ellei ilmoituksen napsautetaan. 
- Tärkeimmät syy, "kuljettama" luvut ovat erilaisia kuin eri kuin "toimitettu" numerot saavuttaa kampanjat, että "sovelluksessa" ja "ulos sovelluksesta" ilmoitukset otetaan huomioon eri tavalla. "Sovelluksessa" ilmoitukset Mobile välitys käsittelemien, mutta "ulos sovelluksesta" ilmoituksia käsitellään ilmoituksen Center laitteen-käyttöjärjestelmässä.

## <a name="push-targeting"></a>Push-kohdistamisen avulla

### <a name="issue"></a>Ongelma
- Sisäänrakennettu kohdistamisen ei toimi odotetulla tavalla.
- Sovelluksen tiedot-tunniste kohdistamisen ei toimi odotetulla tavalla.
- GEO sijainti kohdistamisen ei toimi odotetulla tavalla.
- Kieliasetukset eivät toimi odotetulla tavalla.

### <a name="causes"></a>Syitä

- Varmista, että olet ladannut sovelluksen tiedot-tunnisteet Azure Mobile välitys Käyttöliittymän tai API.
- Rajoittimen push nopeutta tai push-kiintiön sovelluksen tasolla tai rajoittaminen markkinointikampanjan tasolla yleisön voit estää henkilön vastaanottaminen tietyn push, vaikka ne vastaavat muiden kohdentamisesta. 
- "Kieli" on erilainen kuin kohdistamisen perusteella maan tai puhelin sijainti tai GPS sijainnin perusteella kieli, joka on myös erilainen kuin kohdistamisen Geo sijainnin perusteella.
- "Oletuskielen"-viesti lähetetään asiakas, joka ei ole johonkin vaihtoehtoisia kieliä, voit määrittää niiden laitteesta.


## <a name="push-scheduling"></a>Push-ajoitus

### <a name="issue"></a>Ongelma
- Push ajoituksen ei toimi odotetusti (lähetetty liian aikaisin tai viivästyneet).

### <a name="causes"></a>Syitä

- Aikavyöhykkeet voit asioita, jotka ajoitetaan, erityisesti silloin, kun käytät loppukäyttäjän aikavyöhyke.
- Lisäasetukset push-ominaisuuksia voi viivästyä työntää.
- Koska Azure Mobile välitys voi olla pyytää tietoja puhelin-reaaliaikainen ennen lähettämistä push puhelimen asetukset (sijaan sovelluksen tiedot-tunnisteita) viivyttää perustuvan kohdistamisen Vie.
- Kampanjat, jotka on luotu ilman päättymispäivän painallus tallentaa paikallisesti laitteessa ja näyttäminen sovellus avataan, vaikka markkinointikampanja lopetetaan manuaalisesti seuraavan kerran.
- Useamman kuin yhden markkinointikampanjan käynnistäminen samanaikaisesti voi kestää kauemmin Etsi käyttäjä-base (kokeile aloittaaksesi vain yhden markkinointikampanjan kerralla enintään neljä, myös kohde vain aktiiviset käyttäjät niin, että vanha käyttäjien ei tarvitse tarkistaa).
- Jos käytät "Ohita yleisön push lähetetään käyttäjien Ohjelmointirajapinnan kautta"-vaihtoehto Reach-markkinointikampanjan "Markkinointikampanjan"-osassa markkinointikampanja ei Lähetä automaattisesti, sinun on lähettää manuaalisesti saavuttaa Ohjelmointirajapinnan kautta.
- Jos käytät mukautetun luokan Reach-sovellusten ilmoitusten, sinun suoritettava ilmoituksen oikea elinkaaren, tai muuten ilmoituksen ei voi tyhjentää kun käyttäjän hylätä sen.

 
