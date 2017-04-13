<properties
    pageTitle="Azure ominaisuudet"
    description="Opettele varten Pimennys Azure-työkalujen avulla voit määrittää Azure roolin asetukset."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690945.aspx -->

# <a name="azure-role-properties"></a>Azure ominaisuudet #

Azure tehtäväsi eri käyttöasetukset voidaan määrittää sisällä, Pimennys Azure-työkalut.

## <a name="configuring-azure-role-properties"></a>Azure roolin ominaisuuksien määrittäminen ##

Azure-roolin ominaisuuksien määrittäminen tehdään kautta Työntekijä-roolin ominaisuus-valintaikkunat. Avaa pikavalikon roolin Pimennys 's Project Explorer-ruudussa ja valitse **Azure** alivalikosta. (Jos et näe Pimennys Project Explorer-roolin, laajenna Azure projektin Project Explorer.)

![][ic789599]

Tässä ohjeaiheessa on kuvattu eri ominaisuuksia, jotka voit määrittää **Ominaisuudet** -valintaikkunat. Huomaa, että useita ominaisuuksia täytetään automaattisesti kun luot uuden käyttöönottoprojektin Azure.

Ominaisuus-sivut ovat käytettävissä Azure roolit.

* [Virtuaalikoneen ominaisuudet](#virtual_machine_properties)
* [Välimuistin ominaisuudet](#caching_properties)
* [Varmenteet-ominaisuudet](#certificates_properties)
* [Osien ominaisuudet](#components_properties)
* [Virheenkorjaus ominaisuudet](#debugging_properties)
* [Päätepisteet ominaisuudet](#endpoints_properties)
* [Ympäristön muuttujat ominaisuudet](#environment_variables_properties)
* [Kuormituksen tasaaminen / istunnon affiniteetti (a.k.a "muistilappuja istuntojen") ominaisuudet](#session_affinity_properties)
* [Paikallinen tallennus-ominaisuudet](#local_storage_properties)
* [Ominaisuudet: palvelimen asetukset](#server_configuration_properties)
* [SSL purkaminen ominaisuudet](#ssl_offloading_properties)
    
<a name="virtual_machine_properties"></a>
### <a name="virtual-machine-properties"></a>Virtuaalikoneen ominaisuudet ###

Avaa pikavalikon rooli Pimennys 's Project Explorer-ruudussa Valitse **Azure**ja valitse sitten **Ominaisuudet**, ja on mahdollisuus virtuaalikoneen koon muuttaminen ja muuttaa myös kerrat, seuraavassa kuvassa esitetyllä tavalla.

![][ic719499]

>[AZURE.NOTE] Vain Windows: Kun olet määrittänyt esiintymien määrän arvon suurempi kuin 1 ja voit myös määrittää sovelluspalvelin-työkalujen avulla vain 1 roolin esiintymän toimimaan emulaattorin tämän asetuksen arvosta huolimatta. Näin voit välttää sidonta porttiin eri server-esiintymät ristiriitojen (esimerkiksi kaikki yritetään sidonta porttiin 8080) suorittaessaan samassa tietokoneessa. Oman haluamasi esiintymän Laske määrittäminen säilytetään, mutta se tulee voimaan vain, kun otat käyttöön pilveen.

<a name="caching_properties"></a> 
### <a name="caching-properties"></a>Välimuistin ominaisuudet ###

Avaa pikavalikon roolin Pimennys 's Project Explorer-ruudussa ja valitse **Azure** **välimuisti**. Tämän valintaikkunan sisällä voit ottaa nimetyn yhtä sijaitsevat memcache-yhteensopiva tallentaa, joilla voit nopeuttaa web-sovellusten.

![][ic719483]

**Välimuisti** ominaisuutta-sivulla voit määrittää yleisten asetusten seuraavasti:

* onko yhtä sijaitsevat välimuisti käytössä.
* välimuistin koko prosentteina, muistin.
* tallennustilan tilin nimi tallennuksen välimuistin tilan sovelluksen suorittamisen pilvipalveluun tai ei mitään, jos et halua tallentaa välimuistin tila. (Tallennustilan tilin nimi on ei käytössä, kun käynnistät sovelluksen suorittaminen emulaattori.) Jos määrität tallennustilan tilin nimi **(automaattinen)** (joka on myös oletus), välimuistiin kokoonpanosi automaattisesti Käytä saman tallennustilan tilin kuin valitset **Julkaise Azure** -valintaikkunassa.

>[AZURE.NOTE] **(Automaattinen)** -asetus on vain, jos julkaiset Pimennys-työkalujen avulla käyttöönoton ohjattu julkaisutoiminto haluamasi tehoste. Jos sen sijaan voit julkaista .cspkg tiedoston käyttämällä manuaalisesti ulkoisen järjestelmän, kuten [Azure hallinta-portaalin][]käyttöönoton ei toimi oikein.

Seuraavassa valintaikkunassa näet välimuistin ominaisuudet.

![][ic719501]

* **Nimi:** Yhteiskäyttö sijaitsevat välimuistin nimi.
* **Porttinumero:** Porttinumero käyttämään välimuistin.
* **Vanhenemiskäytännön:** Jokin seuraavista arvoista, joka määrittää, kun näppäintä välimuistin vanhenee.
    * **Suora:** Avaimen voimassaolo päättyy, kun **minuuttia live** määrittämässä on saavutettu.
    * **NeverExpires:** Avain ei ole vanheneminen-aika.
    * **SlidingWindow:** Avaimen vanhenee, jos se ei ole on käytetty **minuuttia live**; ajan kuluessa vanhenemisen kellon palautetaan aina, kun sitä käytetään.
* **Minuuttia live:** Kuinka monta minuuttia memcached-näppäimen Live vanhenemiskäytännön veloittaa.
* **Suuri käytettävyys eri roolin esiintymät replikoitua varmuuskopioiden:** Jos käytössä, auttaa suuren käytettävyyden käyttävien replikoida varmuuskopioiden eri roolin esiintymät. Huomaa, että voit käyttää tätä toimintoa käyttöönoton on oltava vähintään kaksi roolin esiintymät voimassa.

Jos haluat lisätä uuden välimuistin, **välimuisti** ominaisuutta-sivulla **Lisää** -painiketta ja **Määritä nimetyt välimuisti** -valintaikkuna avautuu. Anna arvot ominaisuudet, jotka yllä olevien ohjeiden.

Jos haluat muokata nimettyä välimuistin, valitse välimuistin ja **välimuisti** ominaisuutta-sivulla olevaa **Muokkaa** -painiketta. Valintaikkuna avautuu voit muokata välimuisti-ominaisuuksia. Paina **OK** , jos haluat tallentaa välimuisti-arvoja.

Poista välimuistiin, valitse välimuistin ja **välimuisti** ominaisuutta-sivulla olevaa **Poista** -painiketta ja valitse sitten **Kyllä** Vahvista poisto.

Katso lisätietoja siitä, miten voit käyttää välimuistia, [miten voit käyttää Co-located välimuistiin][].

<a name="certificates_properties"></a> 
### <a name="certificates-properties"></a>Varmenteet-ominaisuudet ###

Avaa pikavalikko roolin Pimennys 's Project Explorer-ruudun, valitse **Azure**ja valitse sitten **Varmenteet**.

![][ic710964]

Tämän valintaikkunan sisällä voit lisäät tai poistat Pimennys projektin käyttämät varmenteet. Huomaa, että varmenteet tässä luettelossa ei ole automaattisesti tallennettu minkä tahansa Java keystore sisällä, ja näin ollen eivät ole automaattisesti käytettävissä minkä tahansa käyttöön Java-sovelluksen. He ovat vain rekisteröityjä Azure niin, että he voivat ladattuina Windowsiin näennäiskoneiden käyttöönoton tallentaa ja käyttää myöhemmin muiden Windows-ohjelmiston. Tällä hetkellä vain ominaisuus, joka käyttää varmenteet Viitattu näin **Varmenteet** -valintaikkunan Työkalut, on [SSL purkaminen][], sen riippuvuuden Internet Information Services (IIS) ja sovellus pyytää reititys (ARR), jossa oikea varmennetta saataville tällä tavalla.

Kun otat käyttöön projektin Azure Julkaise ohjatun toiminnon avulla, voit pyydetään osoittavan vastaavat nämä varmenteet sekä salasanansa, jotta voit ladata ne automaattisesti Azure-palveluun, mutta ainoastaan, jos niitä ei ole ladattu on aiemmin henkilökohtaisia tietoja Exchange (PFX)-tiedostoja.

<a name="components_properties"></a> 
### <a name="components-properties"></a>Osien ominaisuudet ###

Avaa pikavalikko roolin Pimennys 's Project Explorer-ruudun, valitse **Azure**ja valitse sitten **osat**. Tämän valintaikkunan sisällä sinulla voi lisätä, muokata, tai poista tehtäväsi osat sekä muuta järjestystä, jossa niitä käsitellään.

![][ic719502]

Osat-ominaisuuden avulla voit lisätä riippuvuudet Azure käyttöönottoprojektin, kuten Java sovelluksen projektit, erityisiä tiedostoja ja suoritettavan komentorivin lausekkeita, jotka ovat tarvitsemia käyttöönoton.

Voit määrittää kunkin osan:

* Huomioon otettavia tuominen Azure käyttöönottoprojektin osa, kun se on muodostettu vaihe.
* Huomioon otettavia käyttöönotto komponentti Azure pilveen vaihe.

>[AZURE.NOTE] Muotoilujen osan tiedostoja tai komento rivit, ota huomioon, että käyttöönoton julkaistaan Windows virtual machine, jotta mukautetut vaiheet on oltava käytössä Windows käyttöjärjestelmää. 

Osat ovat seuraavat ominaisuudet:

* **Tuo:** Menetelmä, joka ilmaisee, miten osan tuodaan projektin kun projekti on luotu. Tämä voi olla jokin seuraavista arvoista:
    * **Kopio:** Osa kopioidaan roolin **approot** kansioon **-** ominaisuudessa määritetyn paikallisen polun.
    * **TYHJENNÄ:** Osa on Java yrityksen arkisto (TYHJENNÄ) **-** ominaisuudessa määritetyn paikallisen polussa sovelluksen Yritysprojektin tuoduista. (Tämä on tunnista automaattisesti projektin kyseisestä sijainnista laatu perusteella työkalujen).
    * **JAR:** Osa on Java-arkisto (JAR) ja tuodaan Java **-** ominaisuudessa määritetyn paikallisen polussa projektista. (Tämä on tunnista automaattisesti projektin kyseisestä sijainnista laatu perusteella työkalujen).
    * **ei mitään:** Voit tuoda osan ei toimenpiteitä. Tämä on käytettävissä, kun osan oletetaan olevan jo esitä roolin **approot** hakemistossa tai osa on pelkästään on suoritettava komentoriviltä komento, **kuin** ominaisuuden **käyttöönotto** -menetelmä ollessa **johto**mukaisesti.
    * **SODAN:** Osa on Java web-sovelluksen arkistona (SODAN) ja dynaaminen Web-projekti **-** ominaisuudessa määritetyn paikallisen polussa tuodaan. (Tämä on tunnista automaattisesti projektin kyseisestä sijainnista laatu perusteella työkalujen).
    * **zip:** Osa on zip-tiedoston ja tuodaan pakkaamisesta kansion tai tiedoston **-** ominaisuudessa määritetyn mukaan.
* **Kohteesta:** Paikallisessa tietokoneessa kansioon tai tiedosto, joka edustaa kohteet tuodaan käyttöönoton lähteen polku. Windows-ympäristömuuttujia voi käyttää tätä ominaisuutta. Kaikki ‑tietuelajille osat tuodaan roolin **approot** kansioon, kun projekti on luotu.
    
    Huomaa, että voi ottaa käyttöön ladata osan otettaessa pilveen (ei laske emulaattori). Lue aiheeseen liittyviä tietoja alla osa lisätään.    
    
* **As:** Tiedostonimi kohdassa osaa voidaan tuoda roolin **approot** kansioon ja Azure pilveen kädessä käyttöön. Jätä tämän ominaisuuden tyhjäksi, jos haluat säilyttää nimi sama kuin paikallisessa tietokoneessa. (Suoritettavan osien, eli käyttäjille, joiden **käyttöönotto** -menetelmä on määritetty **johto**, tämä voi lauseen haluamaansa Windows-komentoriviltä.)

    >[AZURE.IMPORTANT] Jos käytät tätä arvoa välilyöntejä, niitä käsitellään eri tavalla sen mukaan, ota käyttöönottomenetelmää. Jos käyttöönotto-menetelmä on **johto**, välilyönnit tulkitaan komentorivin argumentit erottimiksi ja tiedostonimen osana. Kaikki muut käyttöönotto menetelmiä, välilyönnit tulkitaan tiedostonimen osa.
    
* **Käyttöönotto:** Menetelmä, joka ilmaisee osan toimenpide käyttöönoton käynnistyksen yhteydessä. Tämä voi olla jokin seuraavista arvoista:
    * **Kopio:** Osa kopioidaan **,** -ominaisuudessa määritetyn kohteen polku.
    * **johto:** Osa on suoritettava Windows komentoriviltä lausekkeen kontekstissa **,** -ominaisuudessa määritetyn aikaan käyttöönotto alkaa polun suoritetaan.
    * **ei mitään:** Tarvittavat toimenpiteet otetaan käyttöön osan käyttöönoton käynnistyessä.
    * **zip:** Osa on wsp **,** -ominaisuudessa määritetyn kohteen polku. Tämä tapa on käytettävissä vain, kun **tuonti** -ominaisuus on **zip**.
* **To:** Kohteen path virtuaalikoneen, jossa osa otetaan käyttöön. Windows-ympäristömuuttujia voi käyttää tätä ominaisuutta ja tiedostopolkujen ovat suhteessa **approot**.
    
Jos haluat lisätä uuden osan, **osien** ominaisuutta-sivulla **Lisää** -painiketta ja **Azure rooli-osa** -valintaikkuna avautuu. Anna arvot ominaisuudet, jotka yllä olevien ohjeiden. 

Seuraavassa on esimerkki lisätään uusi SOTA-osa.

![][ic719503]

Kun otat käyttöön pilveen (ei laske emulaattori), jos haluat ottaa käyttöön lataaminen-osaa, varmista, että **cloud sijaan pakkauksen, mukaan lukien-ottaa** on kuitattu. Jos haluat ladata Azure-tallennustilan tilin, valitse tallennustilaan tilin **tallennustilan tilin** -avattavasta luettelosta (Voit muokata mitä on luettelossa **tilit** -linkkiä napsauttamalla), jotka täyttävät osittain **URL-osoite** -kenttään ja täytä jäljellä olevan osuuden URL-osoite. Jos et halua käyttää Azure-tallennustilan, valitse **(ei mitään)** **tallennustilan tilin** avattavasta luettelosta ja kirjoita URL-osoite komponenttisi **URL-osoite** -kenttään. Määritä jollakin seuraavista tavoista:

* **Kopio:** Lataa-osan kopioidaan **,** Kansiopolku määrittämän kohteen polku.
* **saman:** **Käyttöönotto-Lataa** **käyttöönotto paketista**kuin käyttää samaa menetelmää.
* **zip:** Lataa-osa on wsp **,** Kansiopolku määrittämän kohteen polku.

Voit muokata osa, valitse osa ja **osat** ominaisuutta-sivulla **Muokkaa** -painiketta. Valintaikkuna avautuu voit muokata osan ominaisuudet. Paina **OK** , jos haluat tallentaa osan arvoja.

Jos haluat poistaa osan, valitse osa ja **osat** ominaisuutta-sivulla **Poista** -painiketta ja valitse sitten **Kyllä** Vahvista poistaminen.

Osia käsitellään luetellussa järjestyksessä. **Siirrä ylös** - ja **Siirrä alas** -painikkeita käyttämällä järjestyksessä.

>[AZURE.NOTE] Server configuration-toiminto on riippuvainen osat sekä. Kyseisiä osia ei voi poistaa tai muokata poistamatta vastaavan server-määritys. Sinua pyydetään tietoja, jotka, kun yrität muuttaa kyseisten osien.

<a name="debugging_properties"></a> 
### <a name="debugging-properties"></a>Virheenkorjaus ominaisuudet ###

Avaa pikavalikko roolin Pimennys 's Project Explorer-ruudun, valitse **Azure**ja valitse sitten **Virheenkorjaus**. Tämän valintaikkunan sisällä voit voi ottaa käyttöön tai poistaa remote virheenkorjaus, sekä luoda virheenkorjaus käyttömahdollisuudet seuraavassa kuvassa esitetyllä tavalla.

![][ic719504]

Aiheeseen liittyviä tietoja virheenkorjaus on artikkelissa [Azure-sovellusten virheenkorjaus Pimennys][].

<a name="endpoints_properties"></a> 
### <a name="endpoints-properties"></a>Päätepisteet ominaisuudet ###

Avaa pikavalikon roolin Pimennys 's Project Explorer-ruudussa ja valitse **Azure** **päätepisteet**. Tämän valintaikkunan sisällä käyttäjä pystyy päätepisteen, Luo ja muokkaa tai poista seinän päätepiste seuraavassa kuvassa esitetyllä tavalla.

![][ic719505]

Voit lisätä päätepisteen, **päätepisteet** ominaisuuden-sivulla **Lisää** -painiketta ja **Lisää päätepisteen** -valintaikkuna avautuu.

![][ic710897]

Päätepisteen nimi, valitse ( **Input**, **sisäinen**, tai **InstanceInput**) ja määritä julkisista ja yksityisistä portti. Paina **OK** , jos haluat tallentaa uuden päätepisteen arvot.

Päätepisteen tyypin mukaan voi käyttää porttialueiden seuraavasti:

* Päätepisteen ilmauksen esiintymä Julkinen portti voi olla portit (esimerkiksi **2000-2010**) solualueen ja yksityiset portti on kiinteä arvo.
* Sisäinen päätepisteen Julkinen portti ei käytetä, ja yksityiset portti voi olla alueen tai tyhjiksi tai tähti osoittaa, että se on määritetty on automaattisesti määritetty Azure.
* Syötteen päätepisteiden Julkinen portti voi olla vain kiinteä arvo ja yksityinen portti voidaan kiinteä arvo tai tyhjiksi tai arvo tähti ilmaisee, se määritetään automaattisesti Azure.

Jos haluat käyttää sen sijaan, että alueen yhden porttinumero, jätä alueen tyhjä loppuun tekstiruutuun.

Jos portit, jotka on määritetty automaattinen, jos on määritettävä portti käytetään suorituksen aikana, sovellus voi käyttää Azure palvelun suorituksenaikainen-Ohjelmointirajapinta, joka on kuvattu [com.microsoft.windowsazure.serviceruntime paketin yhteenveto][].

Katso, miten esiintymän syötteen päätepisteet voidaan apua virheenkorjaus monen esiintymän käyttöönoton, on artikkelissa [Virheenkorjaus rooli-esiintymän monen esiintymän käyttöönoton][].

Jos haluat muokata päätepistettä, päätepiste ja valitse **päätepisteet** ominaisuuden-sivulla **Muokkaa** -painiketta. Valintaikkuna avautuu voit muokata päätepisteen nimi, tyyppi ja julkisista ja yksityisistä portit. Valitse Tallenna muokattu päätepisteen arvot **OK** .

Poista päätepisteen päätepiste ja **päätepisteet** ominaisuuden-sivulla **Poista** -painiketta ja valitse sitten **Kyllä** Vahvista poisto.

Määrittää joitakin ominaisuuksia (esimerkiksi välimuisti, Remote virheenkorjaus, istunnon affiniteetti tai SSL purkaminen) rooliin käyttäjän käytössä oikein, jotta työkalujen voi määrittää automaattisesti määräten päätepisteet, jotka näkyvät sekä käyttäjän määrittämät päätepisteet. Työkalujen estää käyttäjää muokkaaminen tai poistaminen kuten automaattisesti luotu päätepisteet, kunhan liittyvä ominaisuus on käytössä.

<a name="environment_variables_properties"></a> 
### <a name="environment-variables-properties"></a>Ympäristön muuttujat ominaisuudet ###

Avaa pikavalikko roolin Pimennys 's Project Explorer-ruudun, valitse **Azure**ja valitse sitten **Ympäristömuuttujat**. Tämän valintaikkunan sisällä käyttäjä pystyy luominen ympäristömuuttuja, sekä muuttaa tai poistaa ympäristömuuttuja, seuraavassa kuvassa esitetyllä tavalla.

![][ic719506]

Ympäristömuuttujat ovat käytettävissä käynnistyskomentosarjan rooli käynnistyessä.

>[AZURE.NOTE] Ympäristömuuttujat määritettäessä ottaa huomioon, että käyttöönoton julkaistaan Windows virtual machine, joten ympäristömuuttujat on kelvollinen Windows käyttöjärjestelmää.

Esimerkkinä parhaillaan rooli käynnistyessä ympäristömuuttuja Luo uusi ympäristömuuttuja valitsemalla **Lisää** -painiketta. Seuraavassa näkyy nimeltä **MyRoleVersion** ympäristömuuttuja parhaillaan luodun ja määritetyn arvon **1.0**.

![][ic659268]

Jsp-koodin voi näyttää arvon käyttämällä `System.getenv` menetelmää:

    <body>
      <b> Hello World!</b>
      <p>Running role version: <%= System.getenv("MyRoleVersion") %></p>
    </body>

Tuloksena on tämä tulos, kun sovellus käynnistyy:

![][ic552233]

Voit muokata ympäristömuuttuja, valitsemalla ympäristömuuttuja ja **Ympäristömuuttujat** ominaisuutta-sivulla **Muokkaa** -painiketta. Valintaikkuna avautuu kuin olet ympäristön muuttujan ominaisuuksien muokkaaminen. Valitse **OK** Tallenna ympäristön muuttujien arvoihin.

Poista ympäristömuuttuja, valitse ympäristömuuttuja ja **Ympäristömuuttujat** ominaisuutta-sivulla **Poista** -painiketta ja valitse sitten **Kyllä** Vahvista poisto.

Määrittää joitakin ominaisuuksia (esimerkiksi palvelinkokoonpanon Remote virheenkorjaus tai paikallinen tallennus) rooliin käyttäjän käytössä oikein, jotta työkalujen voi määrittää automaattisesti määräten ympäristömuuttujat, jotka näkyvät sekä käyttäjän määrittämät ympäristömuuttujat. Työkalujen estää käyttäjää muokkaaminen tai poistaminen kuten automaattisesti luotu ympäristömuuttujat, kunhan liittyvä ominaisuus on käytössä.

<a name="session_affinity_properties"></a> 
### <a name="load-balancing--session-affinity-aka-sticky-sessions-properties"></a>Kuormituksen tasaaminen / istunnon affiniteetti (a.k.a "muistilappuja istuntojen") ominaisuudet ###

Avaa pikavalikko roolin Pimennys 's Project Explorer-ruudun, valitse **Azure**ja valitse sitten **Kuormituksen tasaamisen**. Tämän valintaikkunan sisällä käyttäjä pystyy käyttöön ottaminen tai käytöstä istunnon affiniteetti seuraavassa kuvassa esitetyllä tavalla.

![][ic719492]

Aiheeseen liittyviä tietoja on [Istunnon affiniteetti][]. Huomaa myös, tämä ominaisuus toiminta kontekstissa purkaminen SSL-ohjeiden etsiminen [SSL purkaminen][].

<a name="local_storage_properties"></a> 
### <a name="local-storage-properties"></a>Paikallinen tallennus-ominaisuudet ###

Avaa pikavalikko roolin Pimennys 's Project Explorer-ruudun, valitse **Azure**ja valitse sitten **Paikallinen tallennus**. Tämän valintaikkunan sisällä on mahdollisuus luominen, muokkaaminen tai poistaminen, jossa on käytössä sovelluksen virtuaalikoneen paikallisen väliaikaisten. Tiettyjen arvojen voidaan määrittää paikallisen tallennustilan koon sekä onko sisältö säilytetään, kun rooli on kuluessa, seuraavassa kuvassa esitetyllä tavalla.

![][ic719508]

Voit myös määrittää ympäristömuuttuja, joka vastaa paikallisesta säilöstä.

Oletusarvon mukaan kaikki tiedot, jotka voit asentaa Azure on sijoitettu (ja purettuna) roolin esiintymän **approot** -kansiossa. Vaikka useimmat yksinkertainen ominaisuuksissa on riittävästi jopa jälkeen unzipping varattu **approot** hakemiston tilaa on rajoitettu ja hyvin määritetyn (pienempi kuin 1 gt on suositeltavaa kerralla peruslähtökohta). Vuoksi jotta Azure Varaa riittävästi levytilaa suurempi käyttöönotoissa, jotka ehkä sovellu **approot** -kansiossa, sinun kannattaa asettaa paikallisen säilön resurssin **Paikallinen tallennus** -valintaikkunan asetukset. Katso toimintaohjeet helposti, [Käyttöönotto suurissa yrityksissä][].

Voit helposti viitata Käynnistys-komentosarjat (esimerkiksi, että **startup.cmd**) tallennustilan resurssi käyttämällä ympäristömuuttuja automaattisesti liitetty mukaan Pimennys työkalujen resurssin **Paikallinen tallennus** -valintaikkunan esitetyllä tavalla. Kyseisen ympäristömuuttuja sisältää olet määrittänyt käynnistyskomentosarja suoritetaan milloin paikallinen resurssi koko polku. 

Voit muokata Paikallinen tallennus resurssin, paikallinen tallennus resurssi ja **Paikallinen tallennus** ominaisuutta-sivulla **Muokkaa** -painiketta. Valintaikkuna avautuu voit tallentaa paikallisesti resurssin ominaisuuksien muokkaaminen. Valitse Tallenna paikallinen tallennus resurssin arvot **OK** .

Voit poistaa paikallisen säilön resurssin paikallinen tallennus resurssi ja **Paikallinen tallennus** ominaisuutta-sivulla **Poista** -painiketta ja valitse sitten **Kyllä** Vahvista poisto.

<a name="server_configuration_properties"></a> 
### <a name="server-configuration-properties"></a>Ominaisuudet: palvelimen asetukset ###

Avaa pikavalikko roolin Pimennys 's Project Explorer-ruudun, valitse **Azure**ja valitse sitten **Palvelinkokoonpanon**. Tämän valintaikkunan sisällä voivat lisätä, poistaa ja muokata JDK ja Java-sovelluspalvelin käyttämä käyttöönoton, sekä lisäät tai poistat sovelluksia (kuten SOTA, PURKKI tai TYHJENNÄ-tiedostoja) käyttämä käyttöönoton.

### <a name="jdk-configuration"></a>JDK määritys ###

Tämän valintaikkunan avulla voit määrittää JDK paketin käytettävä käyttöönoton. Jos käytät Windows Pimennys, voit määrittää käyttää paikallisesti Azure emulaattorin toimivaa JDK paketin ja voit halutessasi ottaa käyttöön paikallisen asennuksen Azure. -Windows käyttöjärjestelmissä emulaattorin JDK-asetus ei ole käytettävissä ja paikallisesti asennettu JDK ei voi ottaa käyttöön, koska se ei ole yhteensopiva Windowsin kanssa. Kuitenkin riippumatta käyttöjärjestelmille, joita käytät, voit aina valita 3 osapuolen JDK-paketit Azure käyttöön tai osoita Windows-yhteensopiva JDK-paketin vaihtoehtoinen Lataa sijainnista.

Seuraavassa on esimerkki siitä, miten voit määrittää JDK Windows:

![][ic780647]

Jos käytät Windows Pimennys, voit määrittää JDK Laske; emulaattorin käyttäminen Voit tehdä sen varmistamiseksi **käytön tämän tiedoston polun testikäyttöön paikallisesti JDK** on valittuna **emulaattorin käyttöönotto** -osasta. Määritä oman JDK; paikallinen polku Voit siirtyä eri JDK, jos haluat käyttää sitä ei ole valittu automaattisesti. Käytössä on myös asetus, jos haluat ottaa yhteyttä JDK Azure pilvipalvelussa; Voit tehdä sen valitsemalla **käyttöönotto paikallisen JDK (automaattinen – Lataa pilvitallennustilaa)** **Cloud käyttöönotto** -osasta.

Huomautus:-Windows käyttöjärjestelmissä **emulaattorin käyttöönoton** asetukset ja **Ota oma paikallinen JDK** asetus eivät ole käytettävissä. Seuraavassa esimerkissä havainnollistetaan määrittäminen Mac-tietokoneeseen tai muut tuetut-Windows käyttöjärjestelmä JDK:

![][ic789643]

Käyttöjärjestelmä, jota käytät, vaikka sinulla **Cloud käyttöönoton** vaihtoehdoista lähde-ja JDK paketin tyyppi:

* **3 osapuolen JDK paketin käytettävissä Azure käyttöönotto** 
* **Ottaa Mukautettu lataus** 

Jos käytössäsi **3rd osapuolen JDK paketin käytettävissä Azure käyttöönotto** -vaihtoehto:

1. Valitse valintaruutu nimeltä **3rd osapuolen JDK paketin käytettävissä Azure käyttöönotto**.
1. Avattavasta luettelosta Valitse 3, joka on käytettävissä Azure osapuolen JDK-paketti.
1. **JDK** -välilehden näyttää seuraavankaltaiselta Windows:  ![][ic780648] 
    ja se näyttää seuraavankaltaiselta Mac OS-käyttöjärjestelmässä tai muut tuetut-Windows käyttöjärjestelmien: ![][ic789643]
1. Valitse **OK** , Tallenna muutokset.
1. Hyväksy käyttöoikeussopimus 3 osapuolen JDK paketin tarjoajalta pyydettäessä käyttöoikeusehdot. Jos hyväksyt ehdot, valitse **Kyllä** **Hyväksy käyttöoikeussopimus** -valintaikkunassa Sulje.
    Huomaa, jonka kohteet näkyvät **käyttöönotto 3rd osapuolen JDK paketin käytettävissä Azure** -vaihtoehdon avattavasta pohjana logiikan voi mukauttaa. Voit muokata kohteita **JDK** -valintaikkunassa valitsemalla **Mukauta** -linkki. Tämä **JDK** ominaisuutta-sivulla Sulje ja Avaa tiedosto **componentsets.xml** Pimennys, jossa voit muokata haluamallasi tavalla. Ohjeissa **componentsets.xml** sisältyy itse **componentsets.xml** tiedoston.

Jos käytössäsi on **Mukautettu lataus-JDK käyttöönotto** -vaihtoehto:

1. Luo ZIP JDK-asennuksen directory varmistetaan, että itse directory-solmun on alikohde ZIP-rakenteen ja sisällön mukaisesti. Kansion nimeä, huomioi tarvittaessa myöhemmin ja muista JDK asennuksen otetaan käyttöön Windows-virtual machine.
1. Lataa Azure tallennustilan tilille ZIP blob nimellä. Voit tehdä tämän käytön ulkoisesti käytettävissä työkalun lataus BLOB Azure säilöön. On suositeltavaa yksityinen Blob-objektien käyttäminen. Ota seuraavat seikat huomioon Blob-objektien URL-osoitteen ZIP-sisällöstä.
1. Nimetty **käyttöönotto JDK Mukautettu lataus-**valintaruutu.
    Jos haluat ladata Azure-tallennustilan tilin, valitse tallennustilaan tilin **tallennustilan tilin** -avattavasta luettelosta (Voit muokata mitä on luettelossa **tilit** -linkkiä napsauttamalla), jotka täyttävät osittain **URL-osoite** -kenttään ja täytä jäljellä olevan osuuden URL-osoite. Jos et halua käyttää Azure-tallennustilan, valitse **(ei mitään)** **tallennustilan tilin** avattavasta luettelosta ja kirjoita URL-osoite **URL-osoite** -kenttään JDK lataaminen. Jos käytät Azure-tallennustilan, URL-Blob-objektien nimet on kirjoitettava pienellä.
1. Varmista **JAVA_HOME** tekstiruudun viittaa oikea kansionimi. Oletusarvon mukaan se viittaa samaan JDK kansionimen arvona valitsit paikalliseen käyttöön. Mutta jos sisältämien ZIP kansio on eri nimi (esimerkiksi vuoksi käyttämällä eri versiota), Päivitä kansionimen **JAVA_HOME** tekstiruutuun. näin ollen koska asetusta käytetään pilvipalvelussa (eivät sisälly Laske emulaattorin).
1. Valitse **OK** , Tallenna muutokset.

Se on. Nyt kun luot pilveen, huomaat, että paketin koko on paljon pienempi, muodosta tulee yleensä kestää riittävästi aikaa ja käyttöönoton itse julkaistessasi pilveen on otettava riittävästi aikaa. Huomaa, että **Ota käyttöön paikallisessa JDK (automaattinen – Lataa pilvitallennustilaa)** tai **käyttöönotto JDK Mukautettu lataus-** asetukset ovat voimassa vain, kun sovellus otetaan käyttöön pilvessä. Heillä ei vaikuta Laske emulaattorin käyttäjäkokemuksen; osien paikallisen version edelleen käytetään, kun otat käyttöön Laske emulaattori. 

### <a name="server-configuration"></a>Palvelinkokoonpanon ###

Seuraavassa on esimerkki siitä, miten voit määrittää sovelluksen-palvelimeen.

![][ic796926]

Varmista, että **käyttöönotto tämäntyyppisen palvelin** -valintaruutu on valittuna ja valitse sitten haluamasi sovelluspalvelin, jota haluat käyttää.

Argumenteille palvelimen käyttämään pilvessä käyttöönottoa varten, voit hyödyntää seuraavista vaihtoehdoista:

1. **Ota käyttöön 3rd osapuolen server Azure käytettävissä** - tämä on erityisen sovellettavan keskihajonta/testi tilanteissa, jossa käyttöönoton tehokkuuden ja helppokäyttöisyyden on prioriteetti ja palvelin ei edellytä Mukautettu määritys. Tai kun haluat käyttää näiden palvelimia lähtökohtana, mutta voit lisätä haluamasi palvelin mukauttaminen vaiheet koko käyttöönotto käynnistys logiikan.
1. **Mukautettu lataus-käyttöönotto** - tämä on erityisen sovellettavan tuotannon tilanteissa, kun käytössä erityisesti valmiita ja määritetty-palvelin, jota haluat käyttää pilveen.
1. **Ota käyttöön paikallisen palvelimen asennus** - tämä ei-erityisesti käytettävissä, jos paikallinen palvelin-asennus on jo mukautettu määritetty käyttöä varten. Jos valitset tämän vaihtoehdon, paikallisen palvelimen polku on määritettävä myös **paikallisen palvelimen polku** muokkausruutuun.

Jos käytössäsi **3rd osapuolen server Azure käytettävissä käyttöönotto** -vaihtoehto:

1. Valitse valintaruutu nimeltä **3rd osapuolen server Azure käytettävissä käyttöönotto**.
1. Valitse avattavasta valikosta haluamasi Palvelinohjelmisto pilveen käyttöönoton käytettäväksi. Huomautus, jos olet jo määrittänyt tyypin, johon haluat käyttää aiemmin, sinun on rajoitettu valitseminen vain, joka on saman perheen kuin kyseinen palvelintyyppi cloud palvelimen. Mutta jos et valinnut palvelimen tyyppi, voit valita jonkin palvelimissa, jotka ovat tällä hetkellä käytettävissä Azure ja palvelimen lajin valitaan automaattisesti puolestasi.
1. Valitse **OK** , Tallenna muutokset.

Jos **Ota käyttöön mukautettu lataus-** vaihtoehto:

1. Varmista, että olet jo valinnut palvelintyyppi edellisten vaiheiden mukaisesti. Tämä kertoo laajennus ottamisesta käyttöön mukautettu lataaminen, palvelimen, kun se on oltava saman perheen valitun palvelimen tyypiksi.
1. Valitse **Ota käyttöön mukautettu lataus-**niminen-valintaruutu.
    Jos haluat ladata Azure-tallennustilan tilin, valitse tallennustilaan tilin **tallennustilan tilin** -avattavasta luettelosta (Voit muokata mitä on luettelossa **tilit** -linkkiä napsauttamalla), jotka täyttävät osittain **URL-osoite** -kenttään ja täytä URL-Osoitteen jäljellä olevan osuuden palvelimellesi Lataa ZIP (kun käyttämällä Azure tallennus-Blob-objektien nimet URL-osoite on kirjoitettava pienellä). Jos et halua käyttää Azure-tallennustilan, valitse **(ei mitään)** **tallennustilan tilin** avattavasta luettelosta ja kirjoita URL-osoite palvelimen lataaminen ZIP **URL-osoite** -kenttään. ZIP sisältäisi alikansion edustava sovelluksen palvelimen asennuksen hakemistossa. Esimerkiksi jos käytössäsi on varten zip zip sisällä Tomcat Apache 7.0.35, on ali-kansio, joka edustaa asennuksen hakemiston, kuten **apache-tomcat-7.0.35**. 
1. Määritä pääkansion ympäristömuuttuja arvo. Sen oletusarvo on käytettävä paikallisen sovelluspalvelin mahdollinen arvo, mutta voit määrittää arvon, jos cloud sovelluspalvelin eroaa paikallisen sovelluspalvelin. Kuitenkin haluat Varmista, että cloud sovelluspalvelin on saman perheen kuin palvelimen lajin aiemmassa versiossa.
    Jos päivität cloud sovelluksen-palvelin-zip myöhemmin, voit manuaalisesti muokata pääkansion-asetusten tai, jos haluat, että se vastaa uudelleen että paikallinen määrittäminen (Jos olet muuttanut paikallisen sovelluspalvelin liian).
1. Valitse **OK** , Tallenna muutokset.

Voi mukauttaa pohjana logiikan, jonka kohteet näkyvät **Palvelinkokoonpanon** ominaisuutta-sivulla **palvelin** -välilehti. Tämä on lisäominaisuudet, joita saatat tarvita, jos tarpeitasi ulottuu oletusarvot tai jos haluat lisätä muita palvelimiin. Voit mukauttaa funktioiden **palvelimen** -valintaikkuna valitsemalla **Mukauta** -linkkiä. Tämä **Palvelinkokoonpanon** ominaisuutta-sivulla Sulje ja Avaa tiedosto **componentsets.xml** Pimennys, jossa voit muokata tarvittaessa laajentaa server configuration-malli. Ohjeissa **componentsets.xml** sisältyy itse **componentsets.xml** tiedoston.

Jos käytössäsi on **käyttöönotto paikallisen palvelimen (automaattinen – Lataa pilvitallennustilaa)** -vaihtoehto:

1. Valitse valintaruutu nimeltä **käyttöönotto paikallisen palvelimen (automaattinen – Lataa pilvitallennustilaa)**.
1. **Tallennustilan** kohdan avattavasta luettelosta, valitse **(automaattinen)**. Jos määrität tähän **(automaattinen)** , Pimennys työkalujen käyttää saman tallennustilan tilin palvelimen valitsemaasi käyttöönoton **Julkaise Azure** -valintaikkunassa.
1. Valitse **OK** , Tallenna muutokset.

Valitse palvelin-asennuspolku tietokoneeseen **paikallisen palvelimen polku** tekstiruutuun, jos jokin seuraavista ehdoista on tosi:

* Haluat testata käyttöönoton emulaattorin (koskee vain Windows).
* Haluat ottaa käyttöön paikallisesti asennettu palvelimellesi pilveen.
* Haluat käyttää palvelimen mukautetun lataus on omat pilvipalvelussa, jossa kirjainkoko, varmista myös **käyttöönotto paikallisen palvelimen (automaattinen – Lataa pilvitallennustilaa)** -vaihtoehto on valittuna yläpuolella.

Jos mikään edellä olevista koskee työkirjaa, paikallinen palvelin-asetus on valinnainen.

### <a name="applications-configuration"></a>Sovellusten määrittäminen ###

Seuraavassa on esimerkki siitä, miten voit määrittää sovelluksen.

![][ic719512]

Valitse **Lisää** toiseen sovellukseen Lisää tai poista sovellus **poistaminen** . Tehokkuuden parantamiseksi, jos haluat käyttää ladata sovelluksen lähteen otettaessa pilveen, Määritä URL-osoite-tallennustilan tilin jne [osien ominaisuudet](#components_properties) avulla. 

Huhtikuussa 2014 julkaisemisen alkaen sovellustesi automaattisesti ladataan saman tallennustilan huomioon (kohdassa **eclipsedeploy** säilö) kuin se on valittuna. Käyttöönoton käynnistys-logiikkaa sisältää vaihetta, jonka lataa ensin tallennustilan tilin sovellukset. Tämä tarkoittaa, että voi päivittää sovellusten käyttöönoton tarvitsematta uudelleen ja ota uudelleen koko pakkauksen lataamalla sovelluksen suoraan (joko Azure portaalin esimerkiksi) tallennustilan tilin uudempia versioita, SODAN tiedostojen korvaaminen alun perin on ladattu työkalujen avulla. Valitse vain aloittaa kaikki käyttämällä Azure's hallinta-portaalin uudelleen tai kautta komentoriviltä apuohjelmien roolin tapauksissa kierrättäminen. (Käynnistävä kierrättäminen suoraan sisällä Pimennys työkalujen rooli ei tällä hetkellä tueta.)

### <a name="notes-about-server-configuration"></a>Palvelinkokoonpanon huomautuksia ###

**Palvelinkokoonpanon** ominaisuutta-sivulla tehdyt muutokset näkyvät `<component>` package.xml osat.

Kun käytät **ladata automaattisesti...** tai **käyttöönotto Lataa...-** asetuksia JDK tai sovelluspalvelin ja voit rakennuksen pilveen (ei laske emulaattori), ja olet muodostanut yhteyden Internetiin, saatat huomata luominen viestien seuraavanlaisiin Console-tulosteessa kuin Ant muodostimen tarkistaa Lataa käytettävyys:

`[windowsazurepackage] Verifying blob availability (https://example.blob.core.windows.net/temp/tomcat6.zip)...` 

Jos olet valinnut **käyttöönotto Lataa...-** vaihtoehdon, seuraavan varoituksen voi näkyä, mutta Luo säilyvät:

`[windowsazurepackage] warning: Failed to confirm blob availability! Make sure the URL and/or the access key is correct (https://example.blob.core.windows.net/temp/tomcat6.zip).` 

Varoitus on ainoa merkintä Lataa käytettävyys ei ole vahvistettu. Niin Jos käyttöönoton epäonnistuu pilveen jostain syystä, tarkista, jos olet saanut varoitus.

Jos haluat ottaa käytöstä Lataa vahvistus (esimerkiksi silloin, jos se tarpeettoman hidastaa Luo), Määritä `verifydownloads` osoittaa `false` - `<windowsazurepackage>` package.xml elementti: 

`<windowsazurepackage verifydownloads="false" ...>` 

Jos valitsit **ladata automaattisesti...** -vaihtoehdon, valitse konsoli-ikkuna tulee näkyviin muodosta viestien edistymisestä lataus 5 sekunnin välein, kun lataus on tarpeen.

<a name="ssl_offloading_properties"></a> 
### <a name="ssl-offloading-properties"></a>SSL purkaminen ominaisuudet ###

Avaa pikavalikko roolin Pimennys 's Project Explorer-ruudun, valitse **Azure**ja valitse sitten **SSL purkaminen**. 

![][ic719481]

Tämän valintaikkunan sisällä voit ottaa SSL purkaminen, joilla voit helposti tuen ottaminen käyttöön Hypertext Transfer Protocol Secure (HTTPS) Java-ympäristön Azure, valitse tarvitsematta määrittäminen Java sovelluspalvelin SSL. Lisätietoja on artikkelissa [SSL purkaminen][] ja [niiden käyttöä SSL purkaminen][].

## <a name="see-also"></a>Katso myös ##

[Azure työkalujen Pimennys varten][]

[Azure-työkalut asennetaan Pimennys][]

[Pimennys Azure Hei maailma-sovelluksen luominen][]

[Azure projektin ominaisuudet][]

[Azure-tallennustilan tilin luettelo][]

Saat lisätietoja Azure käyttäminen Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure hallinta-portaalissa]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure työkalujen Pimennys varten]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure projektin ominaisuudet]: http://go.microsoft.com/fwlink/?LinkID=699524
[Azure-tallennustilan tilin luettelo]: http://go.microsoft.com/fwlink/?LinkID=699528
[COM.microsoft.windowsazure.serviceruntime paketin yhteenveto]: http://azure.github.io/azure-sdk-for-java/com/microsoft/windowsazure/serviceruntime/package-summary.html
[Pimennys Azure Hei maailma-sovelluksen luominen]: http://go.microsoft.com/fwlink/?LinkID=699533
[Virheenkorjaus rooli-esiintymä monen esiintymän yhdistelmäympäristössä]: http://go.microsoft.com/fwlink/?LinkID=699535#debugging_specific_role_instance
[Virheenkorjaus Pimennys Azure sovellukset]: http://go.microsoft.com/fwlink/?LinkID=699535
[Suurissa yrityksissä käyttöönotto]: http://go.microsoft.com/fwlink/?LinkID=699536
[Voit käyttää yhtä sijaitsevat välimuistiin tallentaminen]: http://go.microsoft.com/fwlink/?LinkID=699542
[Opi käyttämään SSL purkaminen]: http://go.microsoft.com/fwlink/?LinkID=699545
[Azure-työkalut asennetaan Pimennys]: http://go.microsoft.com/fwlink/?LinkId=699546
[Istunnon affiniteetti]: http://go.microsoft.com/fwlink/?LinkID=699548
[SSL-purkaminen]: http://go.microsoft.com/fwlink/?LinkID=699549

<!-- IMG List -->

[ic789599]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789599.png
[ic719499]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719499.png
[ic719483]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719483.png
[ic719501]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719501.png
[ic710964]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic710964.png
[ic719502]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719502.png
[ic719503]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719503.png
[ic719504]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719504.png
[ic719505]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719505.png
[ic710897]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic710897.png
[ic719506]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719506.png
[ic659268]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic659268.png
[ic552233]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic552233.png
[ic719492]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719492.png
[ic719508]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719508.png
[ic780647]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic780647.png
[ic789643]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789643.png
[ic780648]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic780648.png
[ic789643]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789643.png
[ic796926]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic796926.png
[ic719512]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719512.png
[ic719481]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719481.png
