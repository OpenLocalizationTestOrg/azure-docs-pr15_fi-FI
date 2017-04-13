<properties 
    pageTitle="Yrityksen integrointi EDIFACT | Microsoft Azure" 
    description="Opettele käyttämään EDIFACT toimeenpano logiikan sovellusten luominen" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="jeffhollan" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="app-service-logic" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/26/2016" 
    ms.author="jonfan"/>

# <a name="enterprise-integration-with-edifact"></a>Yrityksen EDIFACT integrointi 

> [AZURE.NOTE] Tällä sivulla kerrotaan logiikan sovellusten EDIFACT ominaisuuksia. Saat lisätietoja X12 napsauttamalla [tätä](app-service-logic-enterprise-integration-x12.md).

## <a name="create-an-edifact-agreement"></a>Luo EDIFACT-sopimus 
Ennen kuin voit vaihtaa EDIFACT viestejä, sinun täytyy luoda EDIFACT sopimus ja tallentaa sen integrointi tilisi. Seuraavat vaiheet opastaa luominen EDIFACT sopimuksia.

### <a name="heres-what-you-need-before-you-get-started"></a>Seuraavassa on tarvittavat ennen aloittamista
- Azure-tilaukseesi määritetty [integrointi tili](./app-service-logic-enterprise-integration-accounts.md)  
- Vähintään kaksi [kumppanien](./app-service-logic-enterprise-integration-partners.md) jo määrittänyt tilin integrointi  

>[AZURE.NOTE]Kun luot sopimuksen, sinun saada/lähettäminen ja niistä kumppanin-viestien sisältö on vastattava sopimuksen tyyppi.    


Kun olet [integrointi tilin luoda](./app-service-logic-enterprise-integration-accounts.md) ja [lisätä kumppanit](./app-service-logic-enterprise-integration-partners.md), voit luoda EDIFACT sopimuksia toimimalla seuraavasti:  

### <a name="from-the-azure-portal-home-page"></a>Azure-portaalin kotisivulla

Kun kirjaudut [Azure portal](http://portal.azure.com "Azure portaalissa"):  
1. Valitse vasemmasta valikosta **Selaa** .  

>[AZURE.TIP]**Selaa** -linkki ei ole näkyvissä, jos joudut ehkä Laajenna ensin. Toiminto valitsemalla **Näytä-valikko** -linkki, joka sijaitsee osoitteessa tiivistetyt valikon vasemmasta yläkulmasta.  

![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-0.png)    
2. Kirjoita *integrointi* suodattimen hakuruutuun ja valitse sitten **Integrointi tilit** tulosten luettelosta.       
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-3.png)    
3. Valitse integration-tili, johon luot sopimuksen, joka avaa **Integrointi tilit** -sivu. Jos et näe integraatio tilien luetteloiden [luomaan ensimmäinen](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-4.png)  
4.  Valitse **sopimusten** -ruutu. Jos et näe toimeenpano-ruutu, lisää se ensin.   
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-5.png)     
5. Valitse **Lisää** -painike toimeenpano-sivu, joka avautuu.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-agreement-2.png)  
6. Kirjoita **nimi** sopimuksesi, valitse sitten **sopimuksen tyyppi** EDIFACT, **Host kumppani**, **Host tunnistetiedot**, **Vieras kumppani**, **Vieras tunnistetietojen**toimeenpano-sivu, joka avautuu.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1.png)  
7. Kun olet määrittänyt sopimus-ominaisuudet, valitse **Vastaanottoasetuksia** , voit määrittää, miten vastaanotetuista tekstiviesteistä tämän sopimuksen viestit ovat käsitellään.  
8. Seuraavissa osissa, mukaan lukien tunnukset, Acknowledgment, mallit, ohjausobjektin numeroita, vahvistus, sisäinen asetukset ja eräkäsittely on jaettu vastaanottoasetuksia ohjausobjektin. Määritä nämä ominaisuudet sopimuksen kumppanin kanssa vaihtaminen viestit, joiden perusteella. Näin näkymän ohjausobjektit, määrittää niitä siitä, miten haluat tämän sopimuksen tunnistaa ja käsitellä saapuvien viestien perusteella:  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-2.png)  
9. Tallenna asetukset valitsemalla **OK** .  

### <a name="identifiers"></a>Tunnisteet

|Ominaisuus|Kuvaus |
|---|---|
|UNB6.1 (vastaanottajan viittaus salasana)|Kirjoita aakkosnumeerinen arvo 1-14 merkkien välillä.|
|UNB6.2 (vastaanottajan viittaus valitsin)|Kirjoita aakkosnumeerinen arvo ja merkin vähintään kaksi merkkiä.|

### <a name="acknowledgments"></a>Vahvistus 

|Ominaisuus|Kuvaus |
|----|----|
|Viestin (CONTRL) vastaanottaminen|Valitse tämä valintaruutu, jos palauttaa tekniset (CONTRL) acknowledgment interchange lähettäjälle. Vahvistus lähetetään interchange lähettäjälle sopimuksen Lähetä asetusten perusteella.|
|Hyväksyntä (CONTRL)|Valitse tämä valintaruutu, jos palauttaa toimintojen (CONTRL) acknowledgment interchange lähettäjälle acknowledgment lähetetään interchange lähettäjälle sopimuksen Lähetä asetusten perusteella.|

### <a name="schemas"></a>Rakenteet

|Ominaisuus|Kuvaus |
|----|----|
|UNH2.1 (TYYPPI)|Valitse Määritä tapahtumalaji.|
|UNH2.2 (VERSIO)|Kirjoita viestin versionumero. (Pienin, merkin; enintään kolme merkkiä).|
|UNH2.3 (JULKAISTU VERSIO)|Kirjoita viestin release numero. (Pienin, merkin; enintään kolme merkkiä).|
|UNH2.5 (LIITTYVÄT VARATTUJEN KOODI)|Kirjoita määritetty koodi. (Enintään kuusi merkkiä. On oltava aakkosnumeerinen).|
|UNG2.1 (SOVELLUSTUNNUS LÄHETTÄJÄ)|Kirjoita aakkosnumeerinen arvo vähintään yhden merkin ja 35 merkkiä.|
|UNG2.2 (SOVELLUKSEN LÄHETTÄJÄN KOODIN VALITSIN)|Kirjoita aakkosnumeerinen arvo, jossa on enintään neljä merkkiä.|
|RAKENNE|Valitse aiemmin ladatut rakenteen haluat käyttää liittyvät integrointi-tililtä.|

### <a name="control-numbers"></a>Ohjausobjektin numerot

|Ominaisuus|Kuvaus |
|----|----|
|DISALLOW Interchange ohjausobjektin numero kaksoiskappaleet|Valitse tämä valintaruutu, jos haluat estää kaksoiskappaleiden liittymiä. Jos valittuna, EDIFACT toistaa toimen tarkistaa, että interchange hallinnan numero (UNB5) vastaanotettu musiikkitietojen vastaa aiemmin käsitellyt interchange ohjausobjektin luvun. Jos vastine löytyy, valitse vaihdon ei käsitellä.
|Tarkista kaksoiskappaleiden UNB5 jokaisen (päiviä)|Jos olet valinnut disallow kaksoiskappaleiden interchange ohjausobjektin lukuja, voit määrittää, jolla tarkistus suoritetaan antamalla sopiva **Tarkista kaksoiskappaleiden UNB5 jokaisen (päiviä)** -asetuksen päivien määrän.|
|DISALLOW ryhmän ohjausobjektin numero kaksoiskappaleet|Valitse tämä valintaruutu, jos haluat estää liittymiä kaksoiskappaleiden ryhmän ohjausobjektin lukuja (UNG5).|
|Tapahtuman määrittäminen ohjausobjektin kaksoiskappaleet estäminen|Valitse tämä valintaruutu, jos haluat estää liittymiä kaksoiskappaleiden tapahtuman määrittäminen ohjausobjektin lukuja (UNH1).|
|EDIFACT kuittaus ohjausobjektin numero|Tapahtuman määrittäminen viitenumerot käytettäviksi kuittauksen nimeämään lukuarvon etuliitteen, viittaus lukuja ja siihen.|

### <a name="validations"></a>Vahvistukset

|Ominaisuus|Kuvaus |
|----|----|
|Viestityyppi|Määritä viestin tyyppi. Valmiiksi vahvistus kunkin rivin toiseen automaattisesti lisätään. Jos sääntöjä ei ole määritetty, rivin merkitty oletusarvon käytetään vahvistusta varten.|
|Muokkaa vahvistus|Valitse tämä valintaruutu, jos tekemässä Muokkaa vahvistus tietotyypit määrityksen mukaisesti rakenteen, pituus rajoitukset, tyhjä tietonäkymä elementit ja lopussa olevia erottimet Muokkaa ominaisuuksia.|
|Laajennettu varmennus|Valitse tämä valintaruutu, jos käyttöön laajennettu (XSD) vahvistamisen liittymiä vastaanotettu interchange lähettäjältä. Tämä sisältää kentän pituus, valinnat ja toista Laske lisäksi XSD kelpoisuusehdot tyyppi.|
|Salli alussa tai lopussa olevat nollat|Valitse **Salli** sallimaan alussa ja lopussa olevia nollia; **NotAllowed** Salli alussa tai lopussa olevat nollat tai rajauskohta alussa **rajaaminen** ja lopussa olevat nollat.|
|Rajaa alussa tai lopussa olevat nollat|Valitse tämä valintaruutu, jos haluat leikata minkä tahansa alussa tai lopussa olevat nollat|
|Lopussa erotin-käytäntö|Valitse **Ei sallita** , jos et halua sallia lopussa erottimet ja erottimet liittymän vastaanotettu interchange lähettäjältä. Jos vaihdon lopussa erottimet ja erottimet, se on määritetty virheellinen. Valitse Hyväksy liittymiä kanssa tai ilman lopussa olevia erottimet ja erottimet **Valinnainen** . Jos vastaanotettu interchange on oltava lopussa erottimet ja erottimet, valitse **pakollinen** .|

### <a name="internal-settings"></a>Sisäinen asetukset

|Ominaisuus|Kuvaus |
|----|----|
|Luo tyhjä XML-tunnisteita, jos lopussa erottimet sallitaan|Valitse tämä valintaruutu, jos Sisällytä tyhjä XML-tunnisteita lopussa erottimet interchange lähettäjän.|
|Saapuvien jonottaminen käsittely|Asetuksia ovat muun muassa:</br></br>**Jaa Interchange tapahtuman joukkona - keskeyttää tapahtuman joukot-virhe**: jäsentää jokaisen tapahtuman määrittäminen lisäämällä haluamasi kirjekuoren tapahtuman määrittäminen liittymän eri XML-tiedostoon. Kun tämä asetus, jos vähintään yksi tapahtuman määrittää vaihdon eivät läpäise tarkistusta, sitten vain ne tapahtuman joukot on hyllytetty. Jaa Interchange tapahtuman joukkona - keskeyttää Interchange Format-virhe: jäsentää jokaisen tapahtuman määrittäminen käyttämällä haluamasi kirjekuoren liittymän eri XML-tiedostoon. Kun tämä asetus, jos vähintään yksi tapahtuman määrittää vaihdon eivät läpäise tarkistusta, valitse koko interchange keskeytetään.</br></br>**Säilytä Interchange - keskeyttää tapahtuman joukot-virhe**: jättää vaihdon ennalleen, koko oletusmäärä musiikkitietojen XML-asiakirjan luomisesta. Kun tämä asetus, jos vähintään yksi tapahtuman määrittää vaihdon eivät läpäise tarkistusta, sitten vain ne tapahtuman joukot keskeytetään, kun kaikki tapahtuman joukot käsitellään.</br></br>**Säilytä Interchange - keskeyttää Interchange Format-virhe**: jättää vaihdon ennalleen, koko oletusmäärä musiikkitietojen XML-asiakirjan luomisesta. Kun tämä asetus, jos yhden tai useamman tapahtuman määrittää vaihdon eivät läpäise tarkistusta, valitse koko interchange keskeytetään.|

Sopimuksen on valmis käsittelemään saapuvat viestit, jotka noudattavat valitsemasi asetukset.

Voit määrittää asetukset, jotka kumppaneille lähettämäsi viestit käsitellään seuraavasti:  
10. Valitse **Lähetä asetuksia** voit määrittää, miten tämän sopimuksen kautta lähetetyt viestit ovat käsitellään.  

Lähetyksen asetukset-ohjausobjekti on jaettu seuraavissa osissa, mukaan lukien tunnisteet, Acknowledgment, mallit, Kirjekuoret, merkistöjen ja erottimet, ohjausobjektin numerot ja kelpoisuuden. 

Seuraavassa on näkymän ohjausobjektit. Tee haluamasi valinnat siitä, miten haluat käsitellä kumppaneille tämän sopimuksen kautta lähettämiesi viestien perusteella:   
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-3.png)    
11. Tallenna asetukset valitsemalla **OK** .  

### <a name="identifiers"></a>Tunnisteet
|Ominaisuus|Kuvaus |
|----|----|
|UNB1.2 (syntaksi versio)|Valitse arvo väliltä **1** – **4**.|
|UNB2.3 (lähettäjän käänteisen reititys-osoite)|Kirjoita aakkosnumeerinen arvo vähintään yhden merkin ja 14 merkkiä.|
|UNB3.3 (vastaanottajan käänteisen reititys-osoite)|Kirjoita aakkosnumeerinen arvo vähintään yhden merkin ja 14 merkkiä.|
|UNB6.1 (vastaanottajan viittaus salasana)|Kirjoita aakkosnumeerinen arvo vähintään yksi ja 14 merkkiä.|
|UNB6.2 (vastaanottajan viittaus valitsin)|Kirjoita aakkosnumeerinen arvo ja merkin vähintään kaksi merkkiä.|
|UNB7 (viittaus tunnus)|Kirjoita aakkosnumeerinen arvo vähintään yhden merkin ja 14 merkkiä|

### <a name="acknowledgment"></a>Acknowledgment
|Ominaisuus|Kuvaus |
|----|----|
|Viestin (CONTRL) vastaanottaminen|Valitse tämä valintaruutu, jos isännöityä kumppanin odottaa vastaanottamaan tekniset (CONTRL) acknowledgment. Tämä asetus määrittää isännöidyn kumppani, joka lähettää viestin, pyytää kuittausta Vieras-kumppanilta.|
|Hyväksyntä (CONTRL)|Valitse tämä valintaruutu, jos isännöityä kumppanin odottaa toimintojen (CONTRL) acknowledgment. Tämä asetus määrittää isännöidyn kumppani, joka lähettää viestin, pyytää kuittausta Vieras-kumppanilta.|
|Luo SG1/SG4 silmukan hyväksytyssä tapahtuman joukkojen|Jos valitsit pyytää toimintojen kuittaus, valitse tämä valintaruutu, jos Pakota SG1/SG4 silmukoita sukupolven toimintojen hyväksytyssä tapahtuman joukkojen CONTRL vahvistus.|

### <a name="schemas"></a>Rakenteet
|Ominaisuus|Kuvaus |
|----|----|
|UNH2.1 (TYYPPI)|Valitse Määritä tapahtumalaji.|
|UNH2.2 (VERSIO)|Kirjoita viestin versionumero.|
|UNH2.3 (JULKAISTU VERSIO)|Kirjoita viestin release numero.|
|RAKENNE|Valitse malli, jos haluat käyttää. Rakenteet sijaitsevat integrointi-tilillesi. Käyttämään-rakenteiden linkki integrointi tilisi ensin logiikan sovelluksen.|

### <a name="envelopes"></a>Kirjekuoret
|Ominaisuus|Kuvaus |
|----|----|
|UNB8 (käsittelyn prioriteettikoodi)|Kirjoita aakkosjärjestyksessä arvo, joka ei ole useamman kuin yhden merkin pituinen.|
|UNB10 (viestintä-sopimus)|Kirjoita aakkosnumeerinen arvo vähintään yhden merkin ja 40 merkkiä.|
|UNB11 (testata ilmaisin)|Valitse tämä valintaruutu, jos haluat ilmaista, että luotu interchange testitiedot|
|Käytä UNA segmentin (Service merkkijonon neuvoja)|Valitse tämä valintaruutu, jos UNA segmentin lähetetään musiikkitietojen luomiseen.|
|Käytä UNG osia (funktion ryhmän otsikko)|Valitse tämä valintaruutu, jos ryhmittely osia luominen Vieras kumppanin lähetetyt viestit toiminnalliset ryhmän ylätunnisteessa. Voit luoda UNG osia voi käyttää seuraavat arvot:</br></br>Kirjoita **UNG1**aakkosnumeerinen arvo vähintään yhden merkin ja enintään kuusi merkkiä.</br></br>Kirjoita **UNG2.1**aakkosnumeerinen arvo vähintään yhden merkin ja 35 merkkiä.</br></br>Kirjoita **UNG2.2**aakkosnumeerinen arvo, jossa on enintään neljä merkkiä.</br></br>Kirjoita **UNG3.1**aakkosnumeerinen arvo vähintään yhden merkin ja 35 merkkiä.</br></br>Kirjoita **UNG3.2**aakkosnumeerinen arvo, jossa on enintään neljä merkkiä.</br></br>Kirjoita **UNG6**aakkosnumeerinen arvo vähintään yksi ja enintään kolme merkkiä.</br></br>Kirjoita **UNG7.1**aakkosnumeerinen arvo vähintään yhden merkin ja enintään kolme merkkiä.</br></br>Kirjoita **UNG7.2**aakkosnumeerinen arvo vähintään yhden merkin ja enintään kolme merkkiä.</br></br>Kirjoita **UNG7.3**aakkosnumeerinen arvo 1 merkki vähintään ja 6 merkkiä.</br></br>Kirjoita **UNG8**aakkosnumeerinen arvo vähintään yhden merkin ja 14 merkkiä.|

### <a name="character-sets-and-separators"></a>Merkkien joukot ja erottimet
Muut kuin merkistön, voit kirjoittaa on erilaisia erottimet, jota käytetään kunkin viestityyppi. Jos Merkistö ei ole määritetty viestin rakenteen, merkit käytetään.

|Ominaisuus|Kuvaus |
|----|----|
|UNB1.1 (järjestelmätunnus)|Valitse EDIFACT merkistön voi suojata lähtevän interchange.|
|Rakenne|Valitse rakenne avattavasta luettelosta. Kullakin rivillä on valmiina lisätään uusi rivi. Valitun mallin Valitse Määritä käytettävät erottimet:</br></br>**Osan elementti erotin** – Kirjoita yksi merkki rakenteiset tiedot elementit erotetaan.</br></br>**Tietoja elementti erotin** – Kirjoita yksi merkki erottaa yksinkertaisia elementtejä rakenteiset tiedot osat.</br></br></br></br>**Korvaavan merkki** – Valitse tämä valintaruutu, jos tiedot sisältävät merkkejä, jotka käytetään myös tietojen, segmenttiin tai osan erottimet. Kirjoita korvaamismerkki. Lähtevän viestin EDIFACT luotaessa erotinmerkkejä paketin tiedoista kaikki esiintymät korvataan määritetyn merkin.</br></br>**Osion pääte** – Kirjoita yksi merkki osoittamaan Muokkaa segmentin lopussa.</br></br>**Liitteen** – Valitse merkki, jota käytetään segmentin tunnuksessa on virhe. Jos määrität loppuliite, lisätä pääte segmentin voi olla tyhjä. Jos segmentin pääte on tyhjä, sinun on nimettävä loppuliite.|

### <a name="control-numbers"></a>Ohjausobjektin numerot
|Ominaisuus|Kuvaus |
|----|----|
|UNB5 (ohjausobjektin numero Interchange Format)|Kirjoita etuliite, arvoalue interchange hallinnan numero ja siihen. Nämä arvot käytetään lähtevän interchange luomiseen. Etuliite ja jälkiliite on valinnainen; hallinnan numero on pakollinen. Jokaisen uuden viestin on oma ohjausobjektin numero etuliite ja jälkiliite säilyvät ennallaan.|
|UNG5 (ryhmitellä ohjausobjektin numero)|Kirjoita etuliite, arvoalue interchange hallinnan numero ja siihen. Nämä arvot käytetään Luo ryhmä hallinnan numero. Etuliite ja jälkiliite on valinnainen; hallinnan numero on pakollinen. Hallinnan numero suurenee jokaisen uuden viestin, kunnes suurimman arvon saavutetaan; etuliite ja jälkiliite säilyvät ennallaan.|
|UNH1 (viestin otsikon numero)|Kirjoita etuliite, arvoalue interchange hallinnan numero ja siihen. Luo viestin otsikon Viitenumeroa käytetään nämä arvot. Etuliite ja jälkiliite on valinnainen; Viitenumeroa tarvitaan. Viitenumeroa suurenee jokaisen uuden viestin; etuliite ja jälkiliite säilyvät ennallaan.|

### <a name="validations"></a>Vahvistukset
|Ominaisuus|Kuvaus |
|----|----|
|Viestityyppi|Kun tämä asetus mahdollistaa vahvistuksen vaihdon vastaanottaja. Tämä vahvistus tarkistaa Muokkaa osat-tietotyypit, pituus rajoitukset ja tyhjä tietonäkymä osien vahvistaminen ohjeisiin ja harjoituksiin erottimet tapahtuman määrittäminen tiedot.|
|Muokkaa vahvistus|Valitse tämä valintaruutu, jos tekemässä Muokkaa vahvistus tietotyypit määrityksen mukaisesti rakenteen, pituus rajoitukset, tyhjä tietonäkymä elementit ja lopussa olevia erottimet Muokkaa ominaisuuksia.|
|Laajennettu varmennus|Jos valitset tämän vaihtoehdon ottaa käyttöön laajennetun vahvistamisen liittymiä vastaanotettu interchange lähettäjältä. Tämä sisältää kentän pituus, valinnat ja toista Laske lisäksi XSD kelpoisuusehdot tyyppi. Voit ottaa tunniste vahvistus ilman Muokkaa vahvistus ottaminen käyttöön ja päinvastoin.|
|Salli alussa tai lopussa olevat nollat|Kun tämä asetus määrittää, että vastaanotettu osapuolelta, Muokkaa-interchange ei eivät läpäise tarkistusta Jos Muokkaa-interchange tiedot on osa ei ole sen pituus vaatimus, koska tai lopussa olevat välilyönnit, mutta sen pituus vaatimus vaatimukset, kun ne poistetaan.|
|Rajaa alussa tai lopussa olevat nollat|Kun tämä asetus poistaa alussa ja lopussa olevat nollat.|
|Erottimen lopussa|Kun tämä asetus määrittää vastaanotettu osapuolelta, Muokkaa-interchange ei onnistu vahvistus, jos Muokkaa interchange tiedot on osa ei ole mukainen sen pituus vaatimus (tai lopusta) nollia tai lopussa olevat välilyönnit, mutta sen pituus vaatimus vaatimukset, kun ne poistetaan.</br></br>Valitse **Ei sallita** , jos et halua sallia lopussa erottimet ja erottimet liittymän vastaanotettu interchange lähettäjältä. Jos vaihdon lopussa erottimet ja erottimet, se on määritetty virheellinen.</br></br>Valitse Hyväksy liittymiä kanssa tai ilman lopussa olevia erottimet ja erottimet **Valinnainen** .</br></br>Jos vastaanotettu interchange on oltava lopussa erottimet ja erottimet, valitse **pakollinen** .|

Kun olet valinnut **OK** , valitse Avaa sivu:  
12. Valitse Integration tili-sivu- **sopimusten** -ruutu ja näet juuri lisätyt sopimuksen luettelossa.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-4.png)   

## <a name="learn-more"></a>Opi lisää
- [Lisätietoja yrityksen Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Lisätietoja yrityksen integrointi Pack")  
