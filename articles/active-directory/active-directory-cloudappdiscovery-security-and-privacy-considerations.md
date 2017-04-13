<properties
    pageTitle="Cloud App etsiminen suojaus ja tietosuoja huomioon otettavia seikkoja | Microsoft Azure"
    description="Tässä ohjeaiheessa kerrotaan suojaus ja tietosuoja Cloud App etsiminen liittyvät seikat."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi"/>

# <a name="cloud-app-discovery-security-and-privacy-considerations"></a>Cloud App etsiminen suojaus ja tietosuoja huomioon otettavia seikkoja

Microsoft on sitoutunut suojaamaan yksityisyyttäsi ja tietojen suojaaminen pitäminen ohjelmistoja ja palveluja, jotka helpottavat organisaatiosi tietoturvan hallintaa. <br>
Microsoft tunnistaa, kun antaa muille käyttäjille, tietojen luota edellyttää tiukka suojauksen suunnitteluryhmät sijoitukset ja ammattitaidon takaisin sen.
Microsoft noudattaa tarkka yhteensopivuus ja suojausta koskevia ohjeita-suojatun ohjelmisto development lifecycle käytännöt liiketoiminnan palvelu. <br>
Suojaaminen ja tietojen suojaaminen on Microsoftin yläreunan prioriteetti.

Tässä ohjeaiheessa kerrotaan, miten tietojen kerääminen, käsitteleminen ja alueella Azure Active Directory Cloud App etsiminen




##<a name="overview"></a>Yleiskatsaus

Cloud App etsiminen on Azure AD-ominaisuus ja ylläpidetään Microsoft Azure. <br>
Cloud App etsiminen päätepisteen agentti käytetään sovelluksen etsiminen tietojen keräämiseen hallitun IT-tietokoneissa. <br>
Kerättyjen tietojen lähetetään suojatusti salatun kanavan kautta Azure AD Cloud App etsintä-palveluun. <br>
Organisaation Cloud App etsiminen tiedot sitten näkyy Azure-portaalissa. <br>


<center>![Cloud App etsiminen toiminta](./media/active-directory-cloudappdiscovery-security-and-privacy-considerations/cad01.png)</center> <br>


Seuraavissa osissa seuraa tietoja ja kuvataan, miten se on suojattu siirtyy organisaation Cloud App etsintä-palveluun ja kädessä Cloud App etsintä-portaaliin.



## <a name="collecting-data-from-your-organization"></a>Tietojen kerääminen organisaatiostasi

Voi käyttää saat tietoja organisaatiosi työntekijöiden sovelluksiin Azure Active Directory Cloud App discovery-ominaisuutta, sinun on ensin Azure AD Cloud App etsiminen päätepisteen agentti käyttöön tietokoneissa organisaation.

Azure Active Directory-vuokraajan (tai niiden edustajan) järjestelmänvalvojat voivat ladata agentti asennuspaketin Azure-portaalista. Agentti voi olla manuaalisesti asennettu tai asennettuna useita tietokoneissa organisaation SCCM tai ryhmäkäytännön avulla.

Lisäohjeita käyttöönoton asetukset on on [Cloud App etsiminen ryhmäkäytännön käyttöönotto-oppaassa](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx).
<br>

### <a name="data-collected-by-the-agent"></a>Agentti keräämät tiedot

Agentti kerää tiedot alla olevassa luettelossa, kun yhteys on muodostettu verkkosovellukseen. Tietoja kerätään vain nämä sovellukset, jotka järjestelmänvalvoja on määrittänyt etsinnän. <br>
Voit muokata cloud sovellusluettelossa, agentti valvoo – Microsoft [Azure portal](https://portal.azure.com/)- **asetukset**-kohdassa Cloud App etsintä-sivu->**Tietojen kerääminen**->**sovelluksen Kokoelmaluettelo**. Lisätietoja on artikkelissa [Käytön aloittaminen kanssa Cloud App etsiminen](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)
<br>
**Tietoja luokan**: käyttäjätiedot <br>
**Kuvaus**: <br>
Prosessi, jossa pyynnön tehdyt kohde Web-sovelluksen Windows-käyttäjänimi (esimerkiksi: TOIMIALUE\käyttäjänimi) sekä Windows suojauksen tunnus (SUOJAUSTUNNUS) käyttäjä.


**Tietoja luokan**: tietojen käsittely <br>
**Kuvaus**: <br>
Prosessi, jossa pyynnön kohdalla kohde-Web-sovelluksen nimi (esimerkiksi: "iexplore.exe")

**Tietoja luokan**: tietokoneen tiedot <br>
**Kuvaus**: <br>
Tietokoneen-NetBIOS nimi, johon on asennettu agentti.

**Tietoja luokan**: App liikenne tiedot <br>
**Kuvaus**: <br>

Yhteyden seuraavat tiedot:

- Lähteen (paikallinen tietokone) ja IP-osoitteet ja porttinumerot

- Organisaation, jonka kautta pyyntö menee ulos julkiseen IP-osoite.

- Pyynnön aika

- Liikenne lähettää ja vastaanottaa äänenvoimakkuuden

- IP-versio (4 tai 6)

- TLS-yhteyksien: kohde isäntänimi palvelimen nimi merkintä-tunniste tai palvelimen varmenne.

HTTP seuraavat tiedot:

- Menetelmä (HAE, Kirjaa jne.)

- Protocol (HTTP/1.1 jne.)

- Käyttäjäagentin merkkijono

- Isäntänimi

- Kohteen URI (lukuun ottamatta kyselymerkkijonon)

- Sisältötyypin tiedot

- Referrer URL-tiedot (lukuun ottamatta kyselymerkkijonon)



> [AZURE.NOTE] Edellä mainittujen tietojen HTTP kerätään kaikki muut kuin salatut yhteydet.
Nämä tiedot kirjataan TLS-yhteyksien vain, kun "Tarkan tarkastuksen"-asetus on käytössä-portaalissa. Tämän asetuksen oletusarvo on "Käytössä".
Lisää tiedot, katso alla ja [Käytön aloittaminen kanssa Cloud App etsiminen](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)


Lisäksi agentti verkon tehtävästä keräämiä tietoja se kerää myös anonyymejä tietoja ohjelmisto ja laitekokoonpanon, virheraportit ja tietoja siitä, miten agentti käytetään.

<br><br>
### <a name="how-the-agent-works"></a>Agentti toiminta

Agent-asennus on kaksi osaa:

- Tila osa

- Ydin tilassa ohjaimen osa (Windows suodatus-ympäristö ohjain)



Kun agentti asennetaan tallennetaan tietokoneen kielikohtaiset luotettu varmenteen tietokoneessa, jota ohjelma käyttää sitten suojatun yhteyden Cloud App etsintä-palvelussa. <br>
Agentti hakee säännöllisesti Cloud App etsintäpalvelun käytännön määritys tämän suojatun yhteyden kautta. <br>
Käytäntö on cloud sovellusten näyttölaitteen ja onko automaattinen päivitys olisi otettava käyttöön, muun muassa tietoja.

Kun Web liikenne lähetetään ja vastaanotetaan tietokoneeseen Internet Explorerin ja Chrome-Cloud App etsintä-agentti analysoi liikenne ja hakee asiaa metatiedot (katso yllä **agentti keräämät tiedot** -osio). <br>
Minuutin välein-agentti Lataa kerättyjen metatietojen Cloud App etsintäpalvelun salatun kanavan kautta.

Ohjaimen osa sieppaa salatut liikenteen ja lisää itse salatut muodossa. Lisätietoja **Intercepting tietojen salatut yhteydet (tarkan tarkastuksen)** -osassa.


### <a name="respecting-user-privacy"></a>Arvossa käyttäjän tietosuoja

Tavoitteenamme on antaa järjestelmänvalvojille voit määrittää yksityiskohtaisia nokeentumisen kyselyjä sovellusten käyttö- ja tietosuoja organisaation sopiva tasapaino työkalut. Tämän vuoksi annamme portaalissa asetukset-sivulta seuraavat nupit:

- **Tietojen kerääminen**: Järjestelmänvalvojat voivat halutessaan määrittää, mitkä sovellukset tai luokkia he haluavat etsiminen tietojen käyttämistä varten.

- **Tarkan tarkastuksen**: Järjestelmänvalvojat voivat projektissa Määritä Jos agentti kerää HTTP-liikenne SSL/TLS-yhteyksien (eli **"tarkan tarkastuksen"**). Lisätietoa tästä seuraavan osion.

- **Suostumus asetukset**: Järjestelmänvalvojat voivat käyttää Cloud App etsiminen portaalin Valitse, haluatko Ilmoita käyttäjille tietojen keräys agentti mukaan ja Haluatko edellytä, että käyttäjä hyväksyy ennen kuin käyttäjätietojen keräämisen agentti käynnistyy.

Cloud App etsiminen päätepisteen agentti kerää vain edellä mainittujen **tietojen keräämistä agentti** -osiossa esitettyjä tietoja.


### <a name="intercepting-data-from-encrypted-connections-deep-inspection"></a>Salatut yhteydet (tarkan tarkastuksen) käsittelevät tietoja
Kun mainittiin, järjestelmänvalvojat voivat määrittää agentti tietoja salatut yhteydet ('tarkan tarkastuksen"). TLS ([Transport Layer Security](https://msdn.microsoft.com/library/windows/desktop/aa380516%28v=vs.85%29.aspx)) on yksi yleisimmistä protokollat käytössä Internetissä tänään. Salaamalla TLS tiedonsiirto on jokin muu sähköpostiohjelma muodostaa suojatun ja yksityiset tietoliikenneyhteyden verkkopalvelimeen, jossa TLS olennaiset suojaa kulkeva todennuksen käyttäjätiedot ja estää luottamuksellisten tietojen esittäminen.

Kun lopusta loppuun suojatun salatun kanavan TLS myöntämä mahdollistaa tärkeitä tietoturva ja tietosuoja-protokolla usein väärin haittaohjelmien tai nefarious tarkoitukseen. Paljon niin itse asiassa kyseisen TLS usein kutsutaan "Yleinen palomuuri-ohituksen protokolla". Ongelma ylimmällä on useimmat palomuurit ei voi tutkia TLS viestintä, koska sovelluskerroksen tiedot salataan SSL. Tietää tämä hyökkääjät usein hyödyntää pitää haittaohjelmien paketteja varma siitä, vaikka useimmat älykkäät sovelluskerroksen palomuurit ovat täysin TLS: stä ja on yksinkertaisesti välittää TLS välisessä isännät käyttäjän TLS. Loppukäyttäjät hyödyntää usein TLS ohittamaan toimialuerekisteröintipalvelun niiden yrityksen palomuurien ja välityspalvelinten, access-ohjausobjektit käyttämällä muodostaa julkisen välityspalvelimet ja -TLS tunnelointiprotokollat, joka voi olla estetty muussa käytännön palomuurin läpi.

Tarkan tarkastuksen avulla luotettu mies---keskiosan edustajana Cloud App etsintä-agentti. Kun asiakkaan pyynnön tehdään käyttämään suojattu HTTPS-resurssia, päätepisteen agentti ohjain sieppaa yhteys ja muodostaa uusi yhteys palvelimeen kohde noutaa sen SSL-varmenteen asiakkaan puolesta. Agentti varmistaa, että sertifikaatti luotettu (kun valitset, että se on peruutettu ei ja suorittamiseen varmenteen muut tarkastukset), ja jos pass päätepisteen agentti sitten Kopioi palvelimen sertifikaatin tiedot ja luo oma palvelinvarmennetta--nimeltään kaappaamiselta-sertifikaatin--tietojen käyttäminen. Kaappaamiselta varmenne on allekirjoitettu---lento mukaan päätepisteen agentti pääkansion sertifikaatilla, johon on asennettu luotettu varmenteen Windows-kaupasta. Itse allekirjoitetun sertifikaatti on merkitty ei voi viedä ja on ollut Käyttöoikeusluettelon järjestelmänvalvojille. Se on tarkoitus jättää koskaan tietokoneeseen, johon se on luotu. Kun end-käyttäjän asiakassovellus vastaanottaa kaappaamiselta varmennetta, se luota koska se onnistuneesti Tarkista varmenneketju aivan pääkansion sertifikaatin avulla. Tämä toimenpide on lähinnä läpinäkyvä-käyttäjän näkökulmasta kanssa muutaman huomautukset seuraavalla tavalla.

Ottamalla tarkan tarkastuksen Cloud App etsiminen päätepisteen agentti voit salaus ja tarkasta TLS salattu viestinnän salliminen palvelu ja vähentää vähentämiseksi verovapautta toimittamalla tietoa salattuja cloud-sovellusten käyttö.

#### <a name="a-word-of-caution"></a>Sanan varoitus
Ennen kuin otat käyttöön tarkan tarkastuksen, on suositeltavaa, että viestiä aikeesi oikeudelliset ja HR Osastot ja hankkia niiden suostumusta. Tarkistetaan peruskäyttäjän yksityinen salattuja communications voivat olla luottamuksellisia aihe ilmeisimmät syystä. Tee varma, että yrityksen tietoturvan ennen tuotannon lisensointi laaja tarkastuksen, ja hyväksyttävällä käyttöön käytännöt on päivitetty osoittamaan, että salattua yhteyttä tarkastaa. Käyttäjän ilmoituksen ja poikkeusta sivustojen katsotaan luottamuksellisia (kuten pankki ja lääketieteellinen sivustot) myös olla tarpeen, jos olet määrittänyt Cloud App etsiminen voit seurata niitä. Kuten edellä mainittiin, järjestelmänvalvojat voivat käyttää Cloud App etsiminen portaalin Ilmoita käyttäjille tietojen keräys agentti mukaan ja Haluatko edellyttää käyttäjän lupaa ennen agentti käynnistyy käyttäjätietojen keräämisen.

### <a name="known-issues-and-drawbacks"></a>Tunnetut ongelmat ja haitoista
On joitakin tapauksia, joissa TLS kaappaamiselta vaikuttaa käyttökokemuksesta:

- Laajennettu Validation (EV)-varmenteet hahmonnetaan selaimen osoiterivillä vihreä edustajana osoittaisi, jossa vierailet luotetussa sivustossa. TLS tarkastus et voi kaksoiskappaleet EV ongelmat asiakkaan, jotta varmenteiden käyttävien verkkosivustojen toimivat yleensä varmennetta, mutta osoiteriville eivät näy vihreä.  

- Julkisen avaimen kiinnittäminen (tunnetaan myös nimellä varmenteen kiinnittäminen) on suunniteltu auttaa suojaamaan käyttäjiä-tietojenvaihto ja päivittämättömien varmenteiden myöntäjistä. Kun kiinnitetty sivuston ylimmällä tasolla varmennetta ei vastaa mitään tunnetut hyvä CA-selaimen hylkää virhe yhteys. Koska TLS kaappaamiselta on mahdollista mies---keskiosan, asiakasyhteydet epäonnistuu.

- Jos käyttäjät napsauttavat lukituskuvake on rajattu hiusristikon sisään sivustotiedot selaimen osoite palkki-selaimessa, he eivät näe viimeiset, jolla kirjaudutaan sivuston varmennetta varmenteen myöntäjä yhdistettyjen, mutta sen sijaan, jonka pääte on Windowsin varmenteen yhdistettyjen Luotetut sertifikaatin.

Voit vähentää ongelmaan esiintymää, emme seurantaan pilvipalveluihin ja asiakassovelluksissa verkkosovellusten tunnetut laajennetun vahvistuksen tai julkinen kiinnittäminen ja opasta päätepisteen agentti, voit välttää käsittelevät sellaisiin yhteydet. Myös tällaisissa tapauksissa, saat edelleen cloud nämä sovellukset käyttöön ja siirrettävän tietoja on paljon, mutta ne eivät ole laaja tarkastettu, koska tietoja siitä, miten sovellukset on käytetty ovat käytettävissä.


## <a name="sending-data-to-cloud-app-discovery"></a>Tietojen lähettäminen Cloud App etsiminen

Agentti keräämiä metatietoja, kun se on välimuistiin koneen minuutin tai kunnes välimuistiin tallennetut tiedot saavuttaa koon 5 megatavua. Se on pakattu ja lähettää Cloud App etsintäpalvelun suojatun yhteyden kautta.

Jos agentti ei voi pitää yhteyttä Cloud App etsintäpalvelun jostakin syystä, kerättyjen metatiedot tallennetaan paikallisten tiedostovälimuisti, niitä voi käyttää vain sellaisten käyttäjien tietokoneella (esimerkiksi Järjestelmänvalvojat-ryhmän). <br>
Agentti yrittää automaattisesti lähettää välimuistissa olevia metatietoja, kunnes se onnistuu on vastaanottanut Cloud App etsintäpalvelun.



## <a name="receiving-the-data-at-the-service-end"></a>Palvelun lopussa tietojen vastaanottamiseen

Käyttäjäagenttien todentaa Cloud App etsiminen palvelu koneen tietyn asiakkaan tarkistus-sertifikaatti edellä mainitut ja välittää tietoja salatun kanavan kautta. <br>
Cloud App etsintä-palvelun analytics myyntijakso käsittelee metatietojen kunkin asiakkaan erikseen jakaminen loogisesti analytics putkijohto kaikkien vaiheiden mukaan.
Analysoitujen metatietojen ohjaa eri raportit-portaalissa.

Käsittelemättömän metatiedot ja analysoitujen metatiedot tallennetaan 180 päivää. Lisäksi asiakkaat voivat valita kannattaa tallentaa Azure blob storage tilin haluamaansa analysoitujen metatiedot.
Tästä on hyötyä offline-tilassa analyysi metatietojen sekä pidempi säilytys tiedot.

## <a name="accessing-the-data-using-the-azure-portal"></a>Azure-portaalissa tietojen käyttäminen

Yrittäessäsi suojata kerääminen metatietoja oletusarvoisesti vain Yleiset järjestelmänvalvojat vuokraajan on pääsy Cloud App Discovery-ominaisuutta Azure-portaalissa. <br>
Järjestelmänvalvoja voi kuitenkin halutessaan edustajakäyttöoikeudet muille käyttäjille tai ryhmille.



> [AZURE.NOTE] Lisätietoja on artikkelissa [Käytön aloittaminen kanssa Cloud App etsiminen](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)

<br>
Kuka tahansa käyttäjä, valitse-portaalissa tietojen käyttäminen täytyy olla käyttöoikeus ja Azure AD Premium-käyttöoikeus.



##<a name="additional-resources"></a>Lisäresursseja


* [Miten unsanctioned cloud-sovellukset, joiden avulla voit tutustua oman organisaation sisällä](active-directory-cloudappdiscovery-whatis.md)
* [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
