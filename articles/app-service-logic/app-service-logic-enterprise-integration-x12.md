<properties 
    pageTitle="Yleisiä tietoja X12 ja yrityksen integrointi pakettiin | Microsoft Azure App palvelun | Microsoft Azure" 
    description="Opettele käyttämään X12 sopimusten logiikan-sovellusten luominen" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="app-service-logic" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-x12"></a>Yrityksen X12 integrointi 

>[AZURE.NOTE]Tämän sivun kattaa X12 toiminnot logiikan sovelluksia. Saat lisätietoja EDIFACT napsauttamalla [tätä](app-service-logic-enterprise-integration-edifact.md).

## <a name="create-an-x12-agreement"></a>Luo X12 sopimus 
Ennen kuin voit vaihtaa X12 viestejä, sinun on luotava X12 sopimus ja tallentaa sen integrointi-tilillesi. Seuraavat vaiheet edetään ohjatusti luominen X12 sopimuksen.

### <a name="heres-what-you-need-before-you-get-started"></a>Seuraavassa on tarvittavat ennen aloittamista
- Azure-tilaukseesi määritetty [integrointi tili](./app-service-logic-enterprise-integration-accounts.md)  
- Vähintään kaksi [kumppanien](./app-service-logic-enterprise-integration-partners.md) jo määrittänyt tilin integrointi  

>[AZURE.NOTE]Kun luot sopimuksen, sopimus-tiedoston sisältö on vastattava sopimuksen tyyppi.    


Kun olet [integrointi tilin luoda](./app-service-logic-enterprise-integration-accounts.md) ja [lisätä kumppanit](./app-service-logic-enterprise-integration-partners.md), voit luoda X12 sopimuksen toimimalla seuraavasti:  

### <a name="from-the-azure-portal-home-page"></a>Azure-portaalin kotisivulla

Kun kirjaudut [Azure portal](http://portal.azure.com "Azure portaalissa"):  
1. Valitse vasemmasta valikosta **Selaa** .  

>[AZURE.TIP]**Selaa** -linkki ei ole näkyvissä, jos joudut ehkä Laajenna ensin. Toiminto valitsemalla **Näytä-valikko** -linkki, joka sijaitsee osoitteessa tiivistetyt valikon vasemmasta yläkulmasta.  

![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. Kirjoita *integrointi* suodattimen hakuruutuun ja valitse sitten **Integrointi tilit** tulosten luettelosta.       
![](./media/app-service-logic-enterprise-integration-x12/x12-1-3.png)    
3. Valitse integration-tili, johon luot sopimuksen, joka avaa **Integrointi tilit** -sivu. Jos et näe integraatio tilien luetteloiden [luomaan ensimmäinen](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-x12/x12-1-4.png)  
4.  Valitse **sopimusten** -ruutu. Jos et näe toimeenpano-ruutu, lisää se ensin.   
![](./media/app-service-logic-enterprise-integration-x12/x12-1-5.png)     
5. Valitse **Lisää** -painiketta, joka avaa sopimusten-sivu.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-2.png)  
6. Kirjoita sopimuksesi **nimi** ja valitse sitten **sopimuksen tyyppi**, **Host kumppani**, **Host tunnistetiedot**, **Vieras kumppani**, **Vieras tunnistetietojen**sopimusten-sivu, joka avautuu.  
![](./media/app-service-logic-enterprise-integration-x12/x12-1.png)  
7. Kun olet määrittänyt asetukset vastaanota, valitse **OK** -painike  
Oletetaan, että edelleen:  
8. Valitse **Vastaanottoasetuksia** , voit määrittää, miten vastaanotetuista tekstiviesteistä tämän sopimuksen viestit ovat käsitellään.  
9. Seuraavissa osissa, mukaan lukien tunnukset, Acknowledgment, mallit, Kirjekuoret, ohjausobjektin numeroita, vahvistukset ja sisäinen asetukset on jaettu vastaanottoasetuksia ohjausobjektin. Määritä nämä ominaisuudet sopimuksen kumppanin kanssa vaihtaminen viestit, joiden perusteella. Näin näkymän ohjausobjektit, määrittää niitä siitä, miten haluat tämän sopimuksen tunnistaa ja käsitellä saapuvien viestien perusteella:  
![](./media/app-service-logic-enterprise-integration-x12/x12-2.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-3.png)  
10. Tallenna asetukset valitsemalla **OK** .  

### <a name="identifiers"></a>Tunnisteet

|Ominaisuus|Kuvaus |
|---|---|
|ISA1 (luvan valitsin)|Valitse todennus valitsin-arvo avattavasta luettelosta.|
|ISA2|Valinnainen. Kirjoita luvan tiedot arvo. Jos kirjoitit ISA1 arvo on muu kuin 00, kirjoita vähintään yksi aakkosnumeerisia merkkejä ja enintään 10.|
|ISA3 (suojaus valitsin)|Valitse Suojaus-valitsin-arvo avattavasta luettelosta.|
|ISA4|Valinnainen. Kirjoita tiedot suojaus-arvo. Jos kirjoitit ISA3 arvo on muu kuin 00, kirjoita vähintään yksi aakkosnumeerisia merkkejä ja enintään 10.|

### <a name="acknowledgments"></a>Vahvistus 

|Ominaisuus|Kuvaus |
|----|----|
|Odotettu TA1|Valitse tämä valintaruutu, jos palauttaa tekniset (TA1) acknowledgment interchange lähettäjälle. Nämä vahvistus lähetetään interchange lähettäjälle sopimuksen Lähetä asetusten perusteella.|
|FA oikein|Valitse tämä valintaruutu, jos palauttaa toimintojen (FA) acknowledgment interchange lähettäjälle. Valitse, haluatko 997 tai 999 kuittaussanomien käsitellään rakenteen versiot perusteella. Nämä vahvistus lähetetään interchange lähettäjälle sopimuksen Lähetä asetusten perusteella.|
|Sisällytä AK2/IK2 silmukka|Valitse tämä valintaruutu ottaa käyttöön AK2 silmukoita sukupolven toimintojen vahvistus hyväksytyssä tapahtuman joukkojen. Huomautus: Tämä valintaruutu on käytössä vain, jos olet valinnut oletettu FA-valintaruutu.|

### <a name="schemas"></a>Rakenteet

Valitse rakenne kunkin tapahtumatyyppi (ST1) ja lähettäjä-sovellus (GS2). Vastaanota putkijohto purkaa saapuvan viestin vertaamalla arvot ST1 ja GS2 saapuvan viestin tässä arvoina ja rakenteen saapuvan viestin rakenteen kanssa tässä.

|Ominaisuus|Kuvaus |
|----|----|
|Versio|Valitse X12 versio|
|Tapahtumatyyppi (ST01)|Valitse tapahtumatyyppi|
|Lähettäjä-sovellus (GS02)|Valitse lähettäjä-sovellus|
|Rakenne|Valitse haluamasi us rakennetiedostoon. Rakennetiedostot sijaitsevat integrointi-tilillesi.|

### <a name="envelopes"></a>Kirjekuoret

|Ominaisuus|Kuvaus |
|----|----|
|ISA11 käyttö|Tämän kentän avulla voit määrittää erottimen tapahtuman määrittäminen:</br></br>Valitse käytettävä kymmenjärjestelmän lukuna, vakio tunnus-. " sen sijaan, että saapuvat asiakirjan Muokkaa kymmenjärjestelmän lukuna saa myyntijakso.</br></br>Valitse Määritä yksinkertaisia elementti tai toistuvien tietorakenne toistuvien esiintymää erotinta toiston erotin. Esimerkiksi (^) käytetään yleensä toiston erotin. HIPAA rakenteiden voit käyttää vain sellaisia (^).|

### <a name="control-numbers"></a>Ohjausobjektin numerot

|Ominaisuus|Kuvaus |
|----|----|
|DISALLOW Interchange ohjausobjektin numero kaksoiskappaleet|Valitse tämä vaihtoehto, jos haluat estää kaksoiskappaleiden liittymiä. Jos valittuna, BizTalk palveluiden portaali tarkistaa, että interchange hallinnan numero (ISA13) vastaanotettu musiikkitietojen vastaa interchange hallinnan numero. Jos vastine löytyy, vastaanota putkijohto ei käsittele vaihdon.<br/>Jos olet valinnut disallow kaksoiskappaleiden interchange ohjausobjektin lukuja, voit määrittää, jolla tarkistus suoritetaan antamalla sopiva kaksoiskappaleiden ISA13 Tarkista x päivän välein päivien määrän.|
|DISALLOW ryhmän ohjausobjektin numero kaksoiskappaleet|Valitsemalla tämän vaihtoehdon voit estää liittymiä kaksoiskappaleiden ryhmän ohjausobjektin lukuja.|
|Tapahtuman määrittäminen ohjausobjektin kaksoiskappaleet estäminen|Valitsemalla tämän vaihtoehdon voit estää liittymiä kaksoiskappaleiden tapahtuman määrittäminen ohjausobjektin lukuja.|

### <a name="validations"></a>Vahvistukset

|Ominaisuus|Kuvaus |
|----|----|
|Viestityyppi|Muokkaa viestin laji, kuten 850 ostotilauksen tai 999 käyttöönoton kuittausta.|
|Muokkaa vahvistus|Tarkistaa Muokkaa tietotyypit määrityksen mukaisesti rakenteen, pituus rajoitukset, tyhjä tietonäkymä elementit ja lopussa olevia erottimet Muokkaa ominaisuuksia.|
|Laajennettu varmennus|Jos tietotyyppi ei ole Muokkaa, vahvistus on tietoja elementti vaatimus ja sallittu toiston, luettelointi ja elementti pituus kelpoisuusehdot (min/max).|
|Salli alussa tai lopussa olevat nollat|Lisätilaa ja nolla merkit, jotka ovat alussa tai lopussa olevat säilytetään. Niitä ei poisteta.|
|Lopussa erotin-käytäntö|Luo lopussa erottimet-vastaanotettu interchange. Asetuksia ovat muun muassa NotAllowed, valinnainen ja pakollinen.|

### <a name="internal-settings"></a>Sisäinen asetukset

|Ominaisuus|Kuvaus |
|----|----|
|Muuntaa epäsuora desimaalimuotoon Nn, jos haluat luoda numeerisen arvon 10|Muuntaa Muokkaa, joka on määritetty niin, että muotoilua Nn kantaluku 10 numeerisen arvon BizTalk palvelut-portaalissa keskitason XML-kyselyjä.|
|Luo tyhjä XML-tunnisteita, jos lopussa erottimet sallitaan|Valitse tämä valintaruutu, jos Sisällytä tyhjä XML-tunnisteita lopussa erottimet interchange lähettäjän.|
|Saapuvien jonottaminen käsittely|Jaa Interchange tapahtuman joukkona - keskeyttää tapahtuman joukot-virhe: jäsentää jokaisen tapahtuman määrittäminen lisäämällä haluamasi kirjekuoren tapahtuman määrittäminen liittymän eri XML-tiedostoon. Kun tämä asetus, jos vähintään yksi tapahtuman määrittää vaihdon eivät läpäise tarkistusta, valitse BizTalk Services keskeyttää vain kyseisiä tapahtuman joukkoja. </br></br>Jaa Interchange tapahtuman joukkona - keskeyttää Interchange Format-virhe: jäsentää jokaisen tapahtuman määrittäminen käyttämällä haluamasi kirjekuoren liittymän eri XML-tiedostoon. Kun tämä asetus, jos vähintään yksi tapahtuman määrittää vaihdon eivät läpäise tarkistusta, valitse BizTalk Services keskeyttää koko interchange.</br></br>Säilytä Interchange Format - keskeyttää tapahtuman joukot-virhe: jättää vaihdon ennalleen, koko oletusmäärä musiikkitietojen XML-asiakirjan luomisesta. Kun tämä asetus, jos onAe tai Lisää tapahtuman määrittää vaihdon eivät läpäise tarkistusta, valitse BizTalk Services keskeyttää vain ne tapahtuman joukot-samalla, kun prosessin kaikissa tapahtuman joukoissa.</br></br>Säilytä Interchange Format - keskeyttää Interchange Format-virhe: jättää vaihdon ennalleen, koko oletusmäärä musiikkitietojen XML-asiakirjan luomisesta. Kun tämä asetus, jos vähintään yksi tapahtuman määrittää vaihdon eivät läpäise tarkistusta, valitse BizTalk Services keskeyttää koko interchange.</br></br>|

Sopimuksen on valmis käsittelemään saapuvia viestejä, jotka valitsit rakenteen mukainen.

Voit määrittää asetukset, jotka kumppaneille lähettämäsi viestit käsitellään seuraavasti:  
11. Valitse **Lähetä asetuksia** voit määrittää, miten tämän sopimuksen kautta lähetetyt viestit ovat käsitellään.  

Lähetyksen asetukset-ohjausobjekti on jaettu seuraavissa osissa, mukaan lukien tunnisteet, Acknowledgment, mallit, Kirjekuoret, ohjausobjektin numeroita, merkistöjen ja erottimet ja kelpoisuuden. 

Seuraavassa on näkymän ohjausobjektit. Tee haluamasi valinnat siitä, miten haluat käsitellä kumppaneille tämän sopimuksen kautta lähettämiesi viestien perusteella:   
![](./media/app-service-logic-enterprise-integration-x12/x12-4.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-5.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-6.png)  
12. Tallenna asetukset valitsemalla **OK** .  

### <a name="identifiers"></a>Tunnisteet
|Ominaisuus|Kuvaus |
|----|----|
|Määritin todennus (ISA1)|Valitse todennus valitsin-arvo avattavasta luettelosta.|
|ISA2|Kirjoita luvan tiedot arvo. Jos tämä arvo on muu kuin 00, kirjoita vähintään yksi aakkosnumeerisia merkkejä ja enintään 10.|
|Suojauksen valitsin (ISA3)|Valitse Suojaus-valitsin-arvo avattavasta luettelosta.|
|ISA4|Kirjoita tiedot suojaus-arvo. Jos tämä arvo on muu kuin 00 arvo (ISA4)-kenttään, kirjoita vähintään yksi aakkosnumeerinen arvo ja enintään 10.|

### <a name="acknowledgment"></a>Acknowledgment
|Ominaisuus|Kuvaus |
|----|----|
|Odotettu TA1|Valitse tämä valintaruutu, jos palauttaa tekniset (TA1) acknowledgment interchange lähettäjälle. Tämä asetus määrittää, kuka on viestin lähettäminen host kumppanin pyytää kuittausta sopimuksen Vieras-kumppanilta. Nämä vahvistus odotetaan mukaan host kumppanin vastaanottoasetuksia sopimuksen perusteella.|
|FA oikein|Valitse tämä valintaruutu, jos toimintojen (FA) acknowledgment palaa interchange lähettäjä ja valitse sitten, haluatko 997 tai 999 kuittaussanomien käsitellään rakenteen versiot perusteella. Nämä vahvistus odotetaan mukaan host kumppanin vastaanottoasetuksia sopimuksen perusteella.|
|FA-versio|Valitse FA-versio|

### <a name="schemas"></a>Rakenteet
|Ominaisuus|Kuvaus |
|----|----|
|Versio|Valitse X12 versio|
|Tapahtumatyyppi (ST01)|Valitse tapahtumatyyppi|
|RAKENNE|Valitse malli, jos haluat käyttää. Rakenteet sijaitsevat integrointi-tilillesi. Käyttämään-rakenteiden linkki integrointi tilisi ensin logiikan sovelluksen.|

### <a name="envelopes"></a>Kirjekuoret
|Ominaisuus|Kuvaus |
|----|----|
|ISA11 käyttö|Tämän kentän avulla voit määrittää erottimen tapahtuman määrittäminen:</br></br>Valitse käytettävä kymmenjärjestelmän lukuna, vakio tunnus-. " sen sijaan, että saapuvat asiakirjan Muokkaa kymmenjärjestelmän lukuna saa myyntijakso.</br></br>Valitse Määritä yksinkertaisia elementti tai toistuvien tietorakenne toistuvien esiintymää erotinta toiston erotin. Esimerkiksi (^) käytetään yleensä toiston erotin. HIPAA rakenteiden voit käyttää vain sellaisia (^).</br>|
|Toiston erotin|Kirjoita toiston erotin|
|Ohjausobjektin versionumero (ISA12)|Valitse vakio X12 versio, jota käytetään BizTalk palveluiden portaali luonnissa lähtevän interchange.|
|Käytön ilmaisin (ISA15)|Voiko liittymän yhteydessä on tietoja (I) tuotannon tiedot (P) tai testitiedot (T). Muokkaa vastaanottaa myyntijakso mainostaa tämän ominaisuuden arvoksi yhteydessä.|
|Rakenne|Voit määrittää, miten BizTalk palveluiden portaali Luo GS ja ST osia X12 koodattu interchange, se lähettää Lähetä putkijohto varten.</br></br>Liittää arvot GS1, GS2, GS3, GS4, GS5, GS7 ja GS8 tietojen osat arvoilla tapahtuman tyyppiä ja versio/Release tietojen osat. Kun BizTalk-palveluiden portaali, joka määrittää XML-viestiin on tapahtumatyyppi ja versio/Release osat rivin ruudukon arvot, ja se täyttää lähtevien interchange ruudukon saman rivin arvoilla kirjekuori GS1, GS2, GS3, GS4, GS5, GS7 ja GS8 tietojen elementtien. Tapahtumatyyppi ja versio/Release elementtien arvojen on oltava yksilöllinen.</br></br>Valinnainen. Valitse GS1, toimi koodia arvo avattavasta luettelosta.</br></br>Pakollinen. Kirjoita aakkosnumeerinen arvo GS2-sovelluksen lähettäjän vähintään kaksi merkkiä ja 15 merkkiä.</br></br>Pakollinen. Kirjoita aakkosnumeerinen arvo GS3-sovelluksen-vastaanotin vähintään kaksi merkkiä ja 15 merkkiä.</br></br>Valinnainen. Valitse GS4, CCYYMMDD tai vvkkpp.</br></br>Valinnainen. Valitse GS5, hh: mm, TTMMSS tai HHMMSSdd.</br></br>Valinnainen. Valitse GS7, vastaava keskuksen arvo avattavasta luettelosta.</br></br>Valinnainen. Kirjoita GS8, aakkosnumeerinen arvo on vähintään yksi merkki ja 12 merkkiä tunnistaa asiakirjan.</br></br>**Huomautus**: Nämä ovat arvoja, jotka BizTalk palveluiden portaali Lisää Luo Jos tapahtuman tyyppi interchange GS-kenttiin ja versio/Release elementit samalla rivillä on vastaavuuden vaihdon liittyviä.|

### <a name="control-numbers"></a>Ohjausobjektin numerot
|Ominaisuus|Kuvaus |
|----|----|
|Interchange ohjausobjektin numero (ISA13)|Pakollinen. Kirjoita yhteenlaskeminen BizTalk palveluiden portaali luomisessa lähtevän interchange käyttää interchange hallinnan numero. Kirjoita vähintään 1 ja 999999999 enintään numeerisen arvon.|
|Ohjausobjektin numero (GS06)|Pakollinen. Kirjoita numerot, jotka BizTalk palveluiden portaali käytettävä ryhmän hallinnan numero. Kirjoita vähintään yhden merkin ja yhdeksän merkkiä numeerisen arvon.|
|Tapahtuman määrittäminen ohjausobjektin numero (ST02)|Tapahtuman määrittäminen ohjausobjektin numeron (ST02) Kirjoita solualueelta numeerisia arvoja, keskimmäinen kentät ja aakkosnumeerisia arvoja, valinnainen etuliite ja jälkiliite. Kaikki neljä kenttää on enimmäispituus on yhdeksän merkkiä.|
|Etuliite|Nimeämään numeroalueen tapahtuman määrittäminen ohjausobjektin käytetään kuittauksen Kirjoita ACK ohjausobjektin numero (ST02)-kenttiin. Keskimmäinen kaksi kenttää numeerinen arvo, ja kirjoita aakkosnumeerinen arvo (tarvittaessa) etuliite ja jälkiliite kentät. Keskimmäinen kenttien tarvittavat ja sisältää ohjausobjektin; numero vähimmäis- ja arvot etuliite ja jälkiliite ovat valinnaisia. Kaikkien kolmen kentän enimmäispituus on yhdeksän merkkiä.|
|Jälkiliite|Nimeämään numeroalueen tapahtuman määrittäminen ohjausobjektin käytetään kuittauksen Kirjoita ACK ohjausobjektin numero (ST02)-kenttiin. Keskimmäinen kaksi kenttää numeerinen arvo, ja kirjoita aakkosnumeerinen arvo (tarvittaessa) etuliite ja jälkiliite kentät. Keskimmäinen kenttien tarvittavat ja sisältää ohjausobjektin; numero vähimmäis- ja arvot etuliite ja jälkiliite ovat valinnaisia. Kaikkien kolmen kentän enimmäispituus on yhdeksän merkkiä.|

### <a name="character-sets-and-separators"></a>Merkkien joukot ja erottimet
Muut kuin merkistön, voit kirjoittaa on erilaisia erottimet, jota käytetään kunkin viestityyppi. Jos Merkistö ei ole määritetty viestin rakenteen, merkit käytetään.

|Ominaisuus|Kuvaus |
|----|----|
|Käytettävä merkistö|SELECT X12 merkistön ominaisuuksia, joita kirjoitat sopimuksen vahvistamiseen.</br></br>**Huomautus**: BizTalk palveluiden portaali käyttää tätä asetusta vain vahvistamiseen liittyvät sopimuksen ominaisuuksien arvot. Vastaanota myyntijakso tai Lähetä myyntijakso ohittaa tämän merkistö ominaisuuden suoritettaessa suorituksenaikainen käsittely.|
|Rakenne|Valitse (+)-symboli ja valitse avattavasta luettelosta mallin. Valitun mallin Valitse Määritä käytettävät erottimet:</br></br>Osan elementti erotin – Kirjoita yksi merkki rakenteiset tiedot elementit erotetaan.</br></br>Tietoja elementti erotin – Kirjoita yksi merkki erottaa yksinkertaisia elementtejä rakenteiset tiedot osat.</br></br></br></br>Korvaavan merkki – Valitse tämä valintaruutu, jos tiedot sisältävät merkkejä, jotka käytetään myös tietojen, segmenttiin tai osan erottimet. Kirjoita korvaamismerkki. Lähtevään luotaessa X12-viesti tiedot korvataan määritetyn merkin paketti erotinmerkkejä kaikki esiintymät.</br></br>Määritetään pääte – Kirjoita yksittäistä merkkiä, osoita Muokkaa-osiossa loppuun.</br></br>Jälkiliite – Valitse merkki, jota käytetään segmentin tunnuksessa on virhe. Jos määrität loppuliite, lisätä pääte segmentin voi olla tyhjä. Jos segmentin pääte on tyhjä, sinun on nimettävä loppuliite.|

### <a name="validation"></a>Kelpoisuustarkistus
|Ominaisuus|Kuvaus |
|----|----|
|Viestityyppi|Kun tämä asetus mahdollistaa vahvistuksen vaihdon vastaanottaja. Tämä vahvistus tarkistaa Muokkaa osat-tietotyypit, pituus rajoitukset ja tyhjä tietonäkymä osien vahvistaminen ja lopettavat erottimet tapahtuman määrittäminen tiedot.|
|Muokkaa vahvistus||
|Laajennettu varmennus|Jos valitset tämän vaihtoehdon ottaa käyttöön laajennetun vahvistamisen liittymiä vastaanotettu interchange lähettäjältä. Tämä sisältää kentän pituus, valinnat ja toista Laske lisäksi XSD kelpoisuusehdot tyyppi. Voit ottaa tunniste vahvistus ilman Muokkaa vahvistus ottaminen käyttöön ja päinvastoin.|
|Salli alussa tai lopussa olevat nollat|Kun tämä asetus määrittää, että vastaanotettu osapuolelta, Muokkaa-interchange ei eivät läpäise tarkistusta Jos Muokkaa-interchange tiedot on osa ei ole sen pituus vaatimus, koska tai lopussa olevat välilyönnit, mutta sen pituus vaatimus vaatimukset, kun ne poistetaan.|
|Erottimen lopussa|Kun tämä asetus määrittää vastaanotettu osapuolelta, Muokkaa-interchange ei onnistu vahvistus, jos Muokkaa interchange tiedot on osa ei ole mukainen sen pituus vaatimus (tai lopusta) nollia tai lopussa olevat välilyönnit, mutta sen pituus vaatimus vaatimukset, kun ne poistetaan.</br></br>Valitse ei sallita, jos et halua sallia lopussa erottimet ja erottimet liittymän vastaanotettu interchange lähettäjältä. Jos vaihdon lopussa erottimet ja erottimet, se on määritetty virheellinen.</br></br>Valitse Hyväksy liittymiä kanssa tai ilman lopussa olevia erottimet ja erottimet valinnainen.</br></br>Jos vastaanotettu interchange on oltava lopussa erottimet ja erottimet, valitse pakollinen.|

Kun olet valinnut **OK** , valitse Avaa näiden:  
13. Valitse Integration tili-sivu- **sopimusten** -ruutu ja näet juuri lisätyt sopimuksen luettelossa.  
![](./media/app-service-logic-enterprise-integration-x12/x12-7.png)   

## <a name="learn-more"></a>Opi lisää
- [Lisätietoja yrityksen Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Lisätietoja yrityksen integrointi Pack")  
