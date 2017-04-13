<properties
   pageTitle="Lotus Domino yhdistimen | Microsoft Azure"
   description="Tässä artikkelissa kuvataan Microsoft Lotus Domino yhdistimen määrittämisestä."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="lotus-domino-connector-technical-reference"></a>Lotus Domino yhdistimen tekniset tiedot
Tässä artikkelissa kuvataan Lotus Domino yhdistin. Artikkeli koskee seuraavia tuotteita:

- Microsoftin Käyttäjätietojen hallinta 2016 (MIM2016)
- Forefront käyttäjätietojen hallinta 2010 R2 (FIM2010R2)
    -   Käytä korjaustiedoston 4.1.3671.0 tai uudempi [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 ja FIM2010R2 yhdistin on ladattavissa [Microsoft Download Centeristä](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-lotus-domino-connector"></a>Yleistä Lotus Domino-yhdistin
Lotus Domino yhdistimen avulla voit integroida synkronointipalvelu IBM's Lotus Domino-palvelimen kanssa.

Ylätason näkökulmasta yhdistimen nykyinen versio tukee seuraavia ominaisuuksia:

Toiminto | Tuki
--- | ---
Yhdistetty tietolähde | Palvelin: <li>Lotus Domino 8.5.x</li><li>Lotus Domino 9.x</li>Asiakas:<li>Lotus Notes-9.x</li>
Skenaariot | <li>Objektin elinkaari hallinta</li><li>Ryhmän hallinta</li><li>Salasanojen hallinta</li>
Toiminnot | <li>Koko ja Delta-tuonti</li><li>Vie</li><li>Määrittää ja muuttaa HTTP salasana salasana</li>
Rakenne | <li>Henkilön (Roaming käyttäjän, yhteyshenkilön (henkilöiden todistusta))</li><li>Ryhmän</li><li>Resurssin (resurssi-tilaa, Online-kokouksen)</li><li>Posti-tietokanta</li><li>Dynaaminen etsiminen tuettuja objekteja määritteet</li>

Lotus Domino yhdistimen käyttää Lotus Notes-asiakas vaihtamaan Lotus Domino-palvelimen kanssa. Tämä riippuvuus seurauksena Tuetut Lotus Notes-asiakas on oltava asennettuna synkronointi-palvelimeen. Asiakkaan ja palvelimen välisen suoritetaan Lotus Notes .NET yhteiskäyttöön (Interop.domino.dll)-liittymän kautta. Tämän liittymän helpottaa välisen Microsoft.NET ympäristön ja Lotus Notes-asiakas, ja se tukee pääsy Lotus Domino tiedostoja ja näkymiä. Sama.arvo tuontia varten on myös mahdollista, että C++ alkuperäisen käyttöliittymän käytetään (mukaan valitun delta tuonti-menetelmä).

### <a name="prerequisites"></a>Edellytykset
Ennen kuin käytät yhdistimen, varmista, että sinulla on synkronointi-palvelimeen seuraavasti:

- Microsoft .NET 4.5.2 Framework tai uudempi versio
- Lotus Notes-asiakas on oltava asennettuna synkronointi palvelimeen
- Lotus Domino yhdistimen edellyttää oletusarvon Lotus Domino LDAP rakenteen (schema.nsf) sijaittava Domino hakemisto-palvelimeen. Jos ei ole käytössä, voit asentaa sen käynnissä tai Domino-palvelimeen LDAP-palvelu uudelleen.

### <a name="connected-data-source-permissions"></a>Yhdistetty tietolähteeseen käyttöoikeudet
Suorittaa tuetut tehtäviä Lotus Domino yhdistimen, edellyttää, että seuraavat ryhmän jäsen:

- Koko Access-Järjestelmänvalvojat
- Järjestelmänvalvojat
- Tietokantojen järjestelmänvalvojille

Seuraavassa taulukossa luetellaan kunkin toimintoa varten tarvittavat käyttöoikeudet:

Toiminto | Käyttöoikeudet
--- | ---
Tuo | <li>Julkisten asiakirjojen lukeminen</li><li> Täysi käyttöoikeus järjestelmänvalvoja (kun täydet Järjestelmänvalvojat-ryhmän jäsen, automaattisesti ovat tehokas käyttöoikeus Käyttöoikeusluettelon.)</li>
Vie ja salasanan määrittäminen | Tehokas käyttö: <li>Tiedostojen luominen</li><li>Tiedostojen poistaminen</li><li>Julkisten asiakirjojen lukeminen</li><li>Julkisten asiakirjojen kirjoittaminen</li><li>Toisinto tai kopioi-asiakirjat</li>Vie toimintoja sinun on myös seuraavia rooleja: <li>CreateResource</li><li>GroupCreator</li><li>GroupModifier</li><li>UserCreator</li><li>UserModifier</li>

### <a name="direct-operations-and-adminp"></a>Suora toiminnot ja AdminP
Toimintojen joko siirtyä suoraan Domino-kansio tai AdminP vaiheet. Seuraavat taulukot Luettele kaikki tuetut objektit-toimintojen ja tarvittaessa liittyvät käyttöönotto-menetelmää:

**Ensisijainen osoitteisto**

Objektin | Luo | Päivitys | Poista
--- | --- | --- | ---
Henkilö | AdminP | Suora | AdminP
Ryhmän | AdminP | Suora | AdminP
MailInDB | Suora | Suora | Suora
Resurssi | AdminP | Suora | AdminP

**Toissijainen osoitteisto**

Objektin | Luo | Päivitys | Poista
--- | --- | --- | ---
Henkilö | PUUTTUU | Suora | Suora
Ryhmän | Suora | Suora | Suora
MailInDB | Suora | Suora | Suora
Resurssi | PUUTTUU | PUUTTUU | PUUTTUU

Resurssin luomisen jälkeen muistiinpanojen asiakirja on luotu. Kun resurssi poistetaan, poistetaan vastaavasti Notes-asiakirjaan.

### <a name="ports-and-protocols"></a>Portit ja protokollat
IBM Lotus Notes-asiakas ja Domino-palvelimet viestiä muistiinpanojen Remote toimintosarjan kutsu (NRPC) jossa NRPC käyttää TCP/IP käyttämällä. Portin oletusnumero on 1352, mutta voit muuttaa Domino järjestelmänvalvoja.

### <a name="not-supported"></a>Ei tueta
Lotus Domino yhdistimen nykyinen versio ei tue seuraavia toimintoja:

- Postilaatikon palvelinten välillä siirtyminen

## <a name="create-a-new-connector"></a>Luo uusi yhdistin

### <a name="client-software-installation-and-configuration"></a>Asiakas-ohjelmiston asentaminen ja määrittäminen
Lotus Notes on asennettava palvelimeen **ennen** yhdistin on asennettu.

Kun olet asentanut, varmista, että sinun on tehtävä, **Yksi käyttäjä asentaa**. Oletusarvo **Usean käyttäjän asentaminen** ei toimi.  
![Notes1](./media/active-directory-aadconnectsync-connector-domino/notes1.png)

Valitse ominaisuudet-sivulla Asenna vain pakolliset Lotus Notes-ominaisuudet ja **Asiakkaan yksittäisen kirjautuminen**. Yksittäisen Kirjautuminen vaaditaan yhdistimen voi kirjautua Domino-palvelimeen.  
![Notes2](./media/active-directory-aadconnectsync-connector-domino/notes2.png)

**Huomautus:** Aloita Lotus Notes kerran käyttäjä, joka sijaitsee samaan palvelimeen, jossa voit käyttää yhdistimen palvelutilin tilin. Varmista myös, että Sulje Lotus Notes-asiakas-palvelimeen. Se ei voi olla käytössä samaan aikaan yhdistimen yrittää Domino-palvelimeen.

### <a name="create-connector"></a>Yhdistimen luominen
Luo Lotus Domino yhdistimen **Synkronointipalvelu** Valitse **Hallinta-agentti** ja **Luo**. Valitse yhdistin **Lotus Domino (Microsoft)** .  
![CreateConnector](./media/active-directory-aadconnectsync-connector-domino/createconnector.png)

Jos synkronointipalvelu-versiosi tarjoaa mahdollisuuden määrittää **arkkitehtuuri**, varmista, että sen oletusarvo on määritetty yhdistimen suorittamisen **prosessissa**.

### <a name="connectivity"></a>Yhteys
Valitse yhteys-sivulla Määritä Lotus Domino-palvelimen nimi ja kirjautumistiedot.  
![Yhteys](./media/active-directory-aadconnectsync-connector-domino/connectivity.png)

Domino-palvelimen ominaisuus tukee kahta muotoilua palvelimen nimi:

- Palvelimen nimi
- Palvelimen nimi ja toiminta

**Palvelimen nimi ja toiminta** -muoto on ensisijainen muoto tämän määritteen, koska se sisältää vastauksen yhdistimen yhteyttä Domino-palvelimeen.

Annettu käyttäjätunnus-tiedosto on tallennettu synkronointipalvelu kokoonpanotietokannan.

**Sama.arvo** tuontia on seuraavat vaihtoehdot:

- **Ei mitään**. Yhdistin ei delta tuonnin.
- **Lisää tai päivitä**. Yhdistimen käytettäessä delta tuo lisätään ja päivitetään toimintoja. Poista **Koko** tuontitoiminnon on pakollinen. Tämä toiminto on käytössä .net-yhteiskäyttöön.
- **Lisääminen, päivittäminen ja poistaminen**. Yhdistimen käytettäessä delta tuo lisääminen, päivittäminen ja poistaminen toimintoja. Tämä toiminto on käytössä alkuperäisen C++ liittymät.

Sinulla on **Rakenneasetukset** seuraavista vaihtoehdoista:

- **Oletusarvoinen rakenne**. Yhdistimen havaitsee rakenteen Domino-palvelimesta. Tämä on oletusarvo.
- **DSML rakenne**. Käyttää vain, jos Domino-palvelin ei näy rakenne. Sitten voit luoda rakenteen kanssa DSML-tiedostona ja tuoda sen sijaan. Saat lisätietoja DSML [OASIS](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=dsml).

Kun valitset Seuraava, käyttäjätunnus ja salasana parametrit on vahvistettu.

### <a name="global-parameters"></a>Yleiset parametrit
Yleiset parametrit-sivulla Määritä haluamasi aikavyöhyke ja tuo ja vie-toiminto-vaihtoehto.  
![Yleiset parametrit](./media/active-directory-aadconnectsync-connector-domino/globalparameters.png)

**Domino palvelimen aikavyöhykettä** -parametri määrittää Domino-palvelimen sijainti.

Määritys-asetusta tarvitaan tukemaan **delta tuo** toiminnot, koska sen avulla synkronointi palvelu määrittää kaksi tuonnin muutokset.

#### <a name="import-settings-method"></a>Tuontiasetukset-menetelmällä
**Suorittaa koko tuo mukaan** on vaihtoehdoista:

- Haku
- Näytä (suositus)

**Etsi** käyttää Domino indeksoinnin, mutta se on yleisiä indeksit eivät päivity reaaliajassa, ja palauttaa palvelimesta ei ole aina oikein. Järjestelmän kanssa useita muutoksia, tämä asetus yleensä ei toimi asianmukaisesti ja on false poistaa tietyissä tilanteissa. **Haku** on nopeampaa kuin **näkymä**.

**Näkymä** on suositeltavaa vaihtoehtoa, koska se sisältää oikeat tilan tiedot. On hieman hitaammin haku.

#### <a name="creation-of-virtual-contact-objects"></a>Virtuaalinen yhteyshenkilön objektien luominen
**Luomisen \_yhteyshenkilön objektin** on vaihtoehdoista:

- Ei mitään
- Viite arvot
- Viittaus ja viite arvot

Domino-viittaus määritteet voi olla eri muodoissa, joka viittaa muiden objektien. Voivan edustavat eri muunnelmia, yhdistimen toteuttaa \_yhteyttä objekteja, joita kutsutaan myös **Virtual yhteystiedot** (VC). Nämä objektit on luotu, jotta he voivat liittyä aiemmin ma objekteihin tai suunnitellut uusi objekteina. Tällä tavalla määrite viittaukset voidaan säilyttää.

Ottamalla tämän asetuksen ja jos viittaus-määritteen sisältö ei ole DN-muoto \_yhteyshenkilön objekti luodaan. Ryhmän jäsen määrite voi olla esimerkiksi SMTP-osoitteet. On myös mahdollista shortName ja esitä viittaus määritteiden määritteet. Tässä tapauksessa valitse **Muu-viite arvot**. Tässä määrityksessä on yleisin Domino käyttöotot-asetusta.

Kun Lotus Domino on määritetty on erillinen osoitteistoja eri DN nimillä, joka edustaa samaan objektiin, on mahdollista, voit myös luoda \_objektien Pyydä kaikki viittaus-arvot, jotka löytyvät osoitekirja. Valitse tässä skenaariossa **viittaus ja muu-viite arvot** -vaihtoehto.

Jos sinulla on useita arvoja-määritteen **nimi** Domino-, valitse myös haluat ottaa käyttöön Virtual yhteystietojen luominen niin viittaukset voidaan ratkaista. Esimerkiksi tämän määritteen voi olla useita arvoja avioliiton tai avioeroa jälkeen. Valitse valintaruutu Ota **... Nimi on useita arvoja** tässä tapauksessa.

Yhdistämällä oikea määritteissä \_yhteyshenkilön objektien liittyneet ma objektiin.

Nämä objektit on VC =\_yhteyshenkilö lisätään niiden DN.

#### <a name="import-settings-conflict-object"></a>Tuontiasetukset, ristiriidassa objekti
**Jätä pois ristiriita-objekti**

Suuri Domino-käyttöönoton on mahdollista, että useita objekteja on sama DN replikoinnin ongelmien vuoksi. Tällaisissa tapauksissa yhdistimen näkyy kaksi objektit, joissa on eri UniversalIDs, mutta saman DN. Tämän ristiriidan aiheuttaa yhdistimen tilaa luotava lyhytkestoisia objekti. Yhdistimen voi ohittaa objekteja, jotka on valittu kuin replikoinnin uhrit Domino. Suositus on pitää tämä valintaruutu on valittuna.

#### <a name="export-settings"></a>Vie asetukset
Jos asetus **Käyttää AdminP viittausten päivittämisestä** on valitsematon, vie viittaus määritteitä, kuten jäsen on suora puhelu ja AdminP prosessi ei käytä. Käytä vain tätä vaihtoehtoa, kun AdminP ei ole määritetty säilyttämään viite-eheys.

#### <a name="routing-information"></a>Reititystiedot
Domino on mahdollista, että viittaus-määrite on upotettu loppuliite DN reititystiedot. Esimerkiksi ryhmän jäsen-määrite voi sisältää **CN=example/organization@ABC**. Liitteen @ABC on reititystiedot. Reititystiedot käytetään Domino lähettää sähköpostiviestejä oikea Domino-järjestelmään, mikä voi olla eri organisaatiossa järjestelmän. Voit määrittää yhdistimen alueen organisaatiossa käytetään reititys loppuliitteet reititys tiedot-kenttään. Jos jokin näistä arvoista löytyy loppuliite kuin viittaus-määrite, reititystiedot poistetaan viittaus. Jos viittauksen arvon reititys liitteen ei voi yhdistää johonkin näistä arvoista on määritetty, \_yhteyshenkilön objekti luodaan. Nämä \_yhteyshenkilön objektit luodaan ** RO=@ ** DN lisätty. Näiden \_yhteyshenkilön objektien määritteet lisätään myös Salli liittäminen todellisen objektin tarvittaessa: \_routingName, \_yhteyshenkilön nimi, \_näyttönimi- ja UniversalID.

#### <a name="additional-address-books"></a>Lisää osoitteistoja
Jos sinulla ei ole **neuvoa** asennettu, jossa on toissijainen osoitteistoja nimi, valitse voit syöttää manuaalisesti näiden osoitteistoja.

#### <a name="multivalued-transformation"></a>Moniarvoisen muunnos
Useiden määritteiden Lotus Domino on moniarvoinen. Vastaavat metaverse määritteet ovat yleensä yksittäisen arvon. Määrittämällä tuominen ja vieminen-toiminto-vaihtoehto käyttöön tarvittavien määritteet tarvittavat käännöksen apua yhdistimen.

**Vie**  
Vie toiminnon vaihtoehto tukee tilasta:

- Liitä kohde
- Korvaa kohde

**Korvaa kohde** – kun valitset tämän vaihtoehdon, yhdistimen aina poistaminen Domino määritettä nykyiset arvot, ja niiden tilalle annettujen arvojen. Annettu arvon voi olla yksittäisen arvon tai moniarvoinen.

Esimerkki: Henkilön objektin avustaja-määrite on seuraavat arvot:

- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

Jos uuden avustajan nimeltä **David Alexander** on määritetty tämän henkilön objektin, tulos on:

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf

**Liitä kohde** – kun valitset tämän vaihtoehdon, yhdistimen säilyttää Domino määritteen olemassa olevat arvot ja lisää uudet arvot tiedot luettelon alkuun.

Esimerkki: Henkilön objektin avustaja-määrite on seuraavat arvot:

- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

Jos uuden avustajan nimeltä **David Alexander** on määritetty tämän henkilön objektin, tulos on:

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

**Tuo**  
Tuo toiminnon asetus tukee tilasta:

- Oletusarvo
- Moniarvoisen yksittäisen arvon

**Oletusarvoinen** – kun valitset oletusasetus, kaikista attribuuteista, kaikki arvot ovat tuotu.

**Multivalued yksittäisen arvon** – kun valitset tämän vaihtoehdon, moniarvoinen määritettä muunnetaan yksittäisen arvon määritettä. Jos on olemassa useita arvoja, (tämä on yleensä myös viimeisen) päällimmäisenä arvoa käytetään.

Esimerkki: Henkilön objektin avustaja-määrite on seuraavat arvot:

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

Tämän määritteen viimeisimmän päivityksen on **David Alexander**. Tuo toiminto-asetus on määritetty Multivalued yksittäinen arvo, koska yhdistimen tuodaan **David Alexander** vain connector-tilaa.

Logiikan moniarvoinen määritteet muuntaminen yksittäisen arvon määritteet ei koske ryhmän jäsen-määrite ja henkilön nimi-määrite.

Myös mahdollista määrittää Tuo ja vie muunnos säännöt-määrite, moniarvoisen määritteiden poikkeuksena yleisen säännön. Jos haluat määrittää tämän asetuksen, kirjoita [objektilaji]. [attributename] **pois jätettyjen määrite luettelon tuominen** ja **vieminen pois jätettyjen määrite luettelosta** -ruutuihin. Esimerkiksi jos kirjoitat Person.Assistant ja tuo kaikki arvot on määritetty yleinen merkintää, vain ensimmäinen arvo tuodaan kirjautumisavustajalle.

#### <a name="certifiers"></a>Certifiers
Kaikki organisaation/organisaation yksiköt näkyvät yhdistin. Jos henkilön objektien vieminen ensisijainen osoitteisto, tarvitaan certifier sen salasanalla.

Jos kaikki certifiers on saman salasanan, **kaikki Certifers salasanan** voi käyttää. Voit sitten tähän salasana ja määritä vain certifier-tiedosto.

Jos tuot vain, sinun ei tarvitse määrittää mitään certifiers.

### <a name="configure-provisioning-hierarchy"></a>Määritä valmistelu hierarkia
Kun määrität Lotus Domino-yhdistin, ohittaa tämän valintaikkunan sivun. Lotus Domino-yhdistin ei tue valmistelu hierarkian.  
![Hierarkian valmistelu](./media/active-directory-aadconnectsync-connector-domino/provisioninghierarchy.png)

### <a name="configure-partitions-and-hierarchies"></a>Määritä osiot ja hierarkiat
Kun määrität osiot ja hierarkiat, sinun on valittava ensisijainen kutsutaan NAB=names.nsf osoitteisto. Ensisijainen osoitteiston lisäksi voit valita Toissijainen osoitteistoja löytämääsi.  
![Osiot](./media/active-directory-aadconnectsync-connector-domino/partitions.png)

### <a name="select-attributes"></a>Valitse määritteet
Kun määrität oman määritteitä, sinun on valittava kaikki määritteitä, jotka ovat etuliite ** \_MMS\_**. Nämä määritteet vaaditaan, kun uudet objektit Lotus Domino valmistelu

![Määritteet](./media/active-directory-aadconnectsync-connector-domino/attributes.png)

## <a name="object-lifecycle-management"></a>Objektin elinkaari hallinta
Tässä osassa on yleisiä tietoja Domino eri objektit.

### <a name="person-objects"></a>Henkilö-objektit
Henkilö-objekti edustaa organisaatiossa ja organisaatioyksiköiden käyttäjät. Oletus-määritteet Domino-järjestelmänvalvoja voi lisätä mukautettuja määritteitä henkilön objekti. Henkilö-objekti on oltava vähintään kaikki pakolliset määritteet. Katso luettelo kaikista pakolliset määritteet, [Lotus Notes-ominaisuudet](#lotus-notes-properties). Rekisteröidä henkilön objektin on täytettävä seuraavat vaatimukset:

- Osoitteisto (names.nsf) on on määritetty ja se on ensisijainen osoitteisto.
- Sinulla on oltava O/OU certifier tunnus ja salasana rekisteröidä tietyn käyttäjän organisaation / organisaatioyksikkö.
- Henkilö-objektin ominaisuudet Lotus Notes tietyt on määritettävä. Nämä ominaisuudet käytetään valmistelu henkilön objekti. Lisätietoja on kohdassa eli [Lotus Notes ominaisuudet](#lotus-notes-properties) jäljempänä tässä asiakirjassa.
- Alkuperäinen HTTP salasana henkilö on määrite ja määritä valmistelun aikana.
- Henkilö-objekti on oltava seuraavat kolme tuetut tyyppiä:
    1. Normaali-käyttäjä, jolla on mail-tiedosto ja käyttäjän tunnus-tiedosto
    2. Käyttäjä (Normaali käyttäjä, joka sisältää kaikki sijaitsevan tietokantatiedostot)
    3. Yhteystiedot (käyttäjä ei ole tunnus-tiedoston)

Henkilöiden (lukuun ottamatta yhteyshenkilöt) edelleen voidaan ryhmitellä US käyttäjät ja kansainvälisen käyttäjät määritysten arvon mukaan \_MMS\_IDRegType ominaisuus. Nämä käyttäjät käyttävät Notes-asiakas, voit käyttää Lotus Domino-palvelimet-on huomautukset-tunnus ja henkilön asiakirjan. Jos osallistujat käyttävät muistiinpanojen posti, ne on valmisteltu mail-tiedosto. Käyttäjän on oltava rekisteröity voimaantulo. Lisätietoja on artikkelissa:

- [Muistiinpanojen käyttäjien määrittäminen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_SETTING_UP_NOTES_USERS.html)
- [Käyttäjän rekisteröinti](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_REGISTERING_USERS.html)
- [Käyttäjien hallinta](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_USERS_5151.html)
- [Käyttäjien nimeäminen uudelleen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_USER_AUTOMATICALLY.html)

Näitä toimintoja suoritetaan Lotus Domino ja tuoda sitten synkronointipalvelu.

### <a name="resources-and-rooms"></a>Resurssien ja ryhmät
Resurssi on eri Lotus Domino-tietokantaan. Resurssien voi olla eri tyyppisten laitteita, kuten projektoreissa neuvotteluhuoneiden. Alalajeja Lotus Domino yhdistimen tukemat resurssit, jotka on määritetty resurssilaji-määrite on:

Resurssin laji | Resurssin laji-määrite
--- | ---
Tilaa | 1
Resurssin (muu) | 2
Online-kokoukseen | 3

Resurssin objektityyppi toimimaan tarvitaan:

- Resurssien varaamisen tietokannan pitäisi jo yhdistetty Domino-palvelin
- Sivusto on jo määritetty resurssin

Resurssien varaamiseen-tietokanta sisältää asiakirjojen kolmenlaisia:

- Sivusto-profiili
- Resurssi
- Varaus

Lisätietoja resurssien varaamiseen tietokannan määrittämisestä on artikkelissa [resurssin varauksia tietokannan määrittämisestä](https://www-01.ibm.com/support/knowledgecenter/SSKTMJ_8.0.1/com.ibm.help.domino.admin.doc/DOC/H_SETTING_UP_THE_RESOURCE_RESERVATIONS_DATABASE.html).

**Luo, päivitys- ja poista resursseja**  
Luo, Päivitä ja poista-toiminnot suoritetaan Lotus Domino yhdistin resurssien varaamiseen-tietokannassa. Resurssien luodaan tiedostojen Names.nsf (eli ensisijainen osoitekirja). Saat lisätietoja muokkaaminen ja poistaminen resurssien [muokkaaminen ja poistaminen resurssin asiakirjoja](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_EDITING_AND_DELETING_RESOURCE_DOCUMENTS.html).

**Tuominen ja vieminen resurssien**  
Resurssit voit tuoda ja viedä synkronointipalvelun, kuten muiden objektityyppi. Valitse Objektityyppi resurssiksi määritysten aikana. Viemistä toiminnon on oltava tiedot resurssilaji, kokouksen tietokannan ja sivustonimi.

### <a name="mail-in-databases"></a>Tietokantojen sähköpostit
Posti-tietokanta on tietokannassa, jossa on suunniteltu vastaanottamaan viestejä. Lotus Domino-postilaatikon, joka ei ole liitetty tiettyyn Lotus Domino käyttäjätili on (eli se ei ole omassa tunnuksen tiedoston ja salasanaa). Posti-tietokanta on yksilöllinen käyttäjätunnus ("lyhyt nimi") se ja oma sähköpostiosoite.

Onko tarvitaan erilliset postilaatikko, jossa on oma sähköpostiosoite, johon voidaan jakaa eri käyttäjien keskuudessa (esimerkiksi group@contoso.com), sähköpostit tietokanta on luotu. Tämän postilaatikon käyttöoikeus ohjaa kautta sen Käyttöoikeusluettelon Access Control List (), joka sisältää muistiinpanojen käyttäjät, jotka voivat avata postilaatikon nimet.

Pakolliset määritteet luettelo on nimeltään [Pakolliset määritteet](#mandatory-attributes) tämän artikkelin kohdassa.

Tietokannan tarkoituksena on saada viestin, kun sähköpostit tietokannan asiakirjan luodaan Lotus Domino. Tässä asiakirjassa on oltava jokaiseen palvelimeen, joka tallentaa tietokannan kopion Domino-kansiota. Yksityiskohtainen kuvaus sähköpostit tietokanta-asiakirjan luomisesta on artikkelissa [sähköpostin tietokannan asiakirjan luominen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_A_MAILIN_DATABASE_DOCUMENT_FOR_A_NEW_DATABASE_OVERVIEW.html).

Ennen kuin sähköpostit tietokannan luomiseen tietokanta on jo (olisi pitänyt luoda Lotus järjestelmänvalvoja) Domino-palvelimessa.

### <a name="group-management"></a>Ryhmän hallinta
Saat yksityiskohtaiset yleiskatsaus Lotus Domino-ryhmän hallinta on seuraavissa resursseissa:

- [Ryhmien käyttäminen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_USING_GROUPS_OVER.html)
- [Ryhmän luominen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS_MIDTOPIC_55038956829238418.html)
- [Luovat ja muokkaavat ryhmät](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS.html)
- [Ryhmien hallinta](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_GROUPS_1804.html)
- [Ryhmän nimeäminen uudelleen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_GROUP_STEPS.html)

### <a name="password-management"></a>Salasanojen hallinta
Rekisteröity Lotus Domino, käyttäjän on kahdentyyppisiä salasanat:

1. Käyttäjän salasanan (tallennetut User.id tiedoston)
2. Internet / HTTP-salasana

Lotus Domino yhdistimen tukee vain toimintoja, joiden HTTP-salasana.

Ota käyttöön salasanojen hallinta hallinta agentti suunnittelussa yhdistimen suorittamiseen salasanojen hallinta. Salasanojen hallinta käyttöön valitsemalla **Määritä tunnisteet** valintaikkunan sivulla **käyttöön salasanojen hallinta** .  
![Määritä tunnisteet](./media/active-directory-aadconnectsync-connector-domino/configureextensions.png)

Seuraavat toimenpiteet Internet-salasanan Lotus Domino-yhdistin tuki:

- Aseta salasana: Salasanan asettaminen määrittää uusi HTTP/Internet-salasana Domino käyttäjälle. Oletusarvon mukaan tili on myös lukitus. Poista lukitus-merkinnän näkyvät Sync Engine WMI-liittymän.
- Salasanan muuttaminen: Tässä skenaariossa käyttäjän salasanan vaihtaminen kannattaa tai pyydetään vaihtamaan salasana määrätyn ajan kuluttua. Voit ottaa tämän toiminnon Aseta, sekä (vanha ja uusi salasana) on pakollinen. Kun muuttanut, uusi salasana päivitetään Lotus Domino.

Lisätietoja on artikkelissa:

- [Internet-lukituksen-toiminnon avulla](http://www.ibm.com/developerworks/lotus/library/domino8-lockout/)
- [Internet-salasanojen hallinta](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_NOTES_AND_INTERNET_PASSWORD_SYNCHRONIZATION_7570_OVER.html)

## <a name="reference-information"></a>Lisätietoja
Tässä osassa on luettelo kuten määritteiden kuvauksia ja Lotus Domino yhdistimen määrite koskevat vaatimukset.

### <a name="lotus-notes-properties"></a>Lotus Notes-ominaisuudet
Kun henkilö objektien valmistella Lotus Domino-kansioon, objektit on oltava tietyt ominaisuudet täydennetty arvoilla. Nämä arvot vaaditaan vain Luo toimintoja.

Seuraavassa taulukossa luetellaan nämä ominaisuudet, ja ne kuvaus.

Ominaisuus | Kuvaus
--- | ---
\_MMS_AltFullName | Käyttäjän vaihtoehtoinen koko nimi.
\_MMS_AltFullNameLanguage | Käyttäjän täydellinen vaihtoehtoinen nimi, joka määrittää käytettävä kieli.
\_MMS_CertDaysToExpire | Nykyisen päivämäärän ennen varmenteen päivien määrä vanhenee. Jos ei määritetä, oletusarvo on kahden vuoden nykyisen päivämäärän.
\_MMS_Certifier | Ominaisuus, joka sisältää certifier organisaation hierarkian nimi. Esimerkki: OU = OrganizationUnit, O = organisaatiokaavion, C = maa.
\_MMS_IDPath | Jos ominaisuus on tyhjä, ei ole käyttäjän tunnistetiedot tiedosto luodaan paikallisesti synkronointi-palvelimeen. Jos ominaisuus on tiedostonimi, käyttäjän tunnus-tiedosto luodaan madata-kansioon. Ominaisuus voi sisältää myös koko polku.
\_MMS_IDRegType | Henkilöiden voidaan luokitella yhteystiedot, US käyttäjät ja kansainväliset käyttäjille. Seuraavassa taulukossa on lueteltu mahdolliset arvot: <li>0 - yhteyshenkilö</li><li>1 - fi-käyttäjä</li><li>2 - kansainvälinen käyttäjä</li>
\_MMS_IDStoreType | Pakollinen-ominaisuudeksi US ja kansainväliset käyttäjille. Ominaisuus on kokonaislukuarvo, joka määrittää, onko liitteenä muistiinpanojen osoitteisto tai henkilön mail-tiedosto on tallennettu käyttäjätunnus. Jos Käyttäjätunnus-tiedosto on liitteen osoitteistossa, se voi luoda myös tiedosto, jonka \_MMS_IDPath. <li>Tyhjennä - säilön tunnus tiedoston tunnus säilöön, tunnus-tiedostoa ei ole (käytetään yhteystiedot).</li><li> 1 - liite muistiinpanojen osoitteistossa. \_MMS_Password-ominaisuuden arvoksi on määritettävä käyttäjän tunnistetiedot tiedostoissa, jotka ovat liitteet</li><li>2 - tunnus tallennetaan henkilön Mail-tiedosto. \_MMS_UseAdminP on määritettävä EPÄTOSI kertoaksesi mail-tiedoston voi luoda henkilön rekisteröinnin yhteydessä. \_MMS_Password-ominaisuuden arvoksi on määritettävä tunnistetiedot tiedostoille.</li>
\_MMS_MailQuotaSizeLimit | Megatavua, joita voi sähköposti-tietokanta määrä.
\_MMS_MailQuotaWarningThreshold | Megatavua, joita voi sähköposti-tietokanta ennen varoitetaan määrä.
\_MMS_MailTemplateName | Sähköpostin mallitiedoston, jota käytetään käyttäjän sähköposti-tiedoston luominen. Jos malli on määritetty, Sähköpostitiedosto on luotu käyttämällä määritettyä mallia. Jos mallia ei määritetä, oletusarvo-mallitiedoston käytetään tiedoston luomiseen.
\_MMS_OU | Valinnainen ominaisuus, joka on kohdassa certifier OU nimi. Tämän ominaisuuden pitäisi olla tyhjä yhteystiedot.
\_MMS_Password | Pakollinen-ominaisuudeksi käyttäjille. Ominaisuus on objektin tunnus-tiedostolle salasana.
\_MMS_UseAdminP | Ominaisuuden arvoksi on asetettava tosi, jos mail-tiedosto luodaan AdminP prosessin Domino-palvelimessa (asynkroninen vientiprosessissa). Ominaisuuden arvo on EPÄTOSI, jos mail-tiedosto luodaan Domino-käyttäjän kanssa (synkronisen vientiprosessissa).

Käyttäjän liittyvä tunnus-tiedoston \_MMS_Password-ominaisuus on oltava arvo. Lotus Notes-asiakasohjelman kautta käytön sähköpostin MailServer ja MailFile ominaisuuksien käyttäjän on oltava arvo.

Seuraavat ominaisuudet on oltava käyttämään sähköpostia selaimella arvoja:

- MailFile - pakollinen-ominaisuudeksi, joka sisältää polun Lotus Domino-palvelimeen, johon mail-tiedosto on tallennettu.
- MailServer - pakollinen-ominaisuudeksi, joka sisältää Lotus Domino-palvelimen nimi. Tämä arvo on käytettävä luodessasi Lotus mail-tiedoston Domino-palvelimen nimi.
- HTTPPassword - valinnainen ominaisuus, joka sisältää objektin Web access-salasana.

Sähköpostin ominaisuuksien ilman Domino-palvelimen käyttämiseen HTTPPassword-ominaisuus on oltava arvo. MailFile-ominaisuus ja MailServer ominaisuus voi olla tyhjä.

Kanssa \_MMS_ IDStoreType = 2 (säilön tunnus posti-tiedostossa,) NotesRegistrationclass MailSystem-ominaisuudeksi on määritetty REG_MAILSYSTEM_INOTES (3).

### <a name="mandatory-attributes"></a>Pakolliset määritteet
Lotus Domino yhdistimen tukee pääasiassa tällaisia objekteja (tiedostotyypeistä):

- Ryhmän
- Posti-tietokanta
- Henkilö
- Yhteyshenkilön (henkilö, jolla ei ole certifier)
- Resurssi

Tässä osassa on luettelo attribuuteista, jotka ovat pakollisia kunkin tuetut objektin vieminen Domino-palvelimeen.

Objektityyppi | Pakolliset määritteet
--- | ---
Ryhmän | <li>Luettelonimi</li>
Pää-tietokanta | <li>Nimi</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li>
Henkilö | <li>Sukunimi</li><li>MailFile</li><li>ShortName</li><li>\_MMS_Password</li><li>\_MMS_IDStoreType</li><li>\_MMS_Certifier</li><li>\_MMS_IDRegType</li><li>\_MMS_UseAdminP</li>
Yhteyshenkilön (henkilö, jolla ei ole certifier) | <li>\_MMS_IDRegType</li>
Resurssi | <li>Nimi</li><li>ResourceType</li><li>ConfDB</li><li>Resurssikapasiteetin</li><li>Sivuston</li><li>Näyttönimi</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li>

## <a name="common-issues-and-questions"></a>Yleisiä asentamisongelmat ja kysymykset

### <a name="schema-detection-does-not-work"></a>Rakenteen tunnistus ei toimi
Voi tunnistaa rakennetta, on tarpeen, että schema.nsf-tiedosto on olemassa Domino-palvelimeen. Tämä tiedosto on näkyvissä vain, jos LDAP-palvelimeen on asennettu. Jos rakenne ei ole havaittavissa, tarkista seuraavat asiat:

- Tiedoston schema.nsf ei sisällä tietoja Domino-palvelimen pääkansiossa
- Käyttäjän on schema.nsf-tiedoston käyttöoikeudet.
- Voit pakottaa LDAP-palvelimen uudelleen. Avaa **Lotus Domino-konsolin** ja Lataa malli **Kertoa LDAP ReloadSchema** -komennon avulla.

### <a name="not-all-secondary-address-books-are-visible"></a>Kaikki toissijainen osoitteistot näkyvät
Domino-yhdistin on riippuvainen ominaisuus **Neuvoa** löydät toissijainen osoitteistoja. Jos toissijainen osoitteistoja puuttuvat, tarkistaa, onko [Neuvoa](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_DIRECTORY_ASSISTANCE.html) käytössä ja määritetty Domino-palvelimeen.

### <a name="custom-attributes-in-domino"></a>Mukautetut määritteet Domino
Domino laajenna rakenne, jotta se näkyy mukautetun määritteen kulutettavia yhdistin on useita tapoja.

**Vaihtoehto 1: Laajentaa Lotus Domino rakenne**

1. Luoda kopion Domino Directory-malli {PUBNAMES. NTF} seuraavat [seuraavasti](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (sinun olisi ei mukauttaminen IBM Lotus Domino oletuskansio malli) mukaan:
2. Avaa Domino kopioi kansion malli {CONTOSO. NTF}-malli, joka on luotu Domino suunnittelussa ja toimi seuraavasti:
    - Valitse jaettu osat ja laajenna alilomakkeita
    - Kaksoisnapsauta ${nimi} InheritableSchema alilomakkeen (jossa {nimi} on oletusarvoinen rakenteellisia objekti-luokan nimi esimerkiksi: henkilö).
    - Haluat lisätä rakenteen {MyPersonAtrribute} ja vastaavat määritteen määritteen nimi Luo kenttä valitsemalla **Luo** ja valitse sitten valikosta **kenttä** .
    - Määritä lisätyn kentän ominaisuuksia valitsemalla sen tyyppiä, tyyliä, kokoa, fonttia ja muut Aiheeseen liittyvät parametrit kentän ominaisuudet-ikkunassa.
    - Säilytä määritteen oletusarvo sama kuin, määritteen nimi (esimerkiksi jos määritteen nimi on MyPersonAttribute, pitää saman niminen oletusarvo).
    - Tallenna ${nimi} InheritableSchema alilomakkeen päivitetyt arvot.
3. Korvaa Domino Directory-malli {PUBNAMES. NTF} uuden mukautetun mallin {CONTOSO. NTF} seuraavat [näiden ohjeiden](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html)mukaan.
4. Domino järjestelmänvalvojan Sulje ja avaa Domino-konsolin LDAP-palvelun käynnistäminen uudelleen ja lataa LDAP-rakennetta:
    - Lisää Domino-konsolin **Domino-komennon** teksti tallennettu käynnistämään LDAP-palvelu - [Uudelleen tehtävän LDAP]( http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html)-komennosta.
    - Lataa LDAP rakenteen komennolla kertoa LDAP - kertoa LDAP-ReloadSchema
5. Avaa Domino-järjestelmänvalvoja ja valitse henkilöt ja ryhmät-välilehdessä näet lisätty määrite näkyy domino Lisää henkilö.
6. Avaa Schema.nsf **tiedostot** -välilehti ja katso lisätty määrite näkyy yhdeksi dominoPerson LDAP objektiluokka.

**Menetelmä 2: Luo mukautettu määrite auxClass ja liittää objektiluokka**

1. Luoda kopion Domino Directory-malli {PUBNAMES. NTF} suorittamalla seuraavat [seuraavasti](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (ei koskaan mukauttaminen IBM Lotus Domino oletuskansio malli):
2. Avaa Domino kopioi kansion malli {CONTOSO. NTF}-malli, joka on luotu Domino suunnittelussa.
3. Valitse vasemmanpuoleisessa ruudussa jaettu koodi ja alilomaketta.
4. Valitse uuden alilomakkeen
5. Määritä uuden alilomakkeen ominaisuudet seuraavasti:
    - Uuden alilomakkeen Avaa, valitse rakenne - alilomakkeen ominaisuudet
    - Viereen Name-ominaisuutta Aux objektiluokka – esimerkiksi TestSubform nimi.
    - Säilytä asetukset-ominaisuutta "Sisällyttäminen Lisää alilomakkeen... valintaikkunan" valittuna
    - Poista asetukset ominaisuuden "hahmontaminen vaiheen kautta HTML muistiinpanojen."
    - Jätä muut ominaisuudet sama ja sulje alilomake ominaisuudet-ruutu.
    - Tallenna ja sulje uuden alilomakkeen.
6. Voit määrittää Aux objektiluokka-kentän lisääminen seuraavasti:
    - Avaa luomasi alilomakkeen.
    - Valitse Luo - kenttään.
    - Kenttä-valintaikkunassa perustiedot-välilehdessä nimen vieressä määrittää minkä tahansa nimen, esimerkiksi: {MyPersonTestAttribute}.
    - Määritä lisätyn kentän ominaisuuksia valitsemalla sen tyyppi, tyyliä, kokoa, fonttia ja ominaisuuksiin.
    - Säilytä määritteen oletusarvo sama kuin, määritteen nimi (esimerkiksi jos määritteen nimi on MyPersonTestAttribute, pitää saman niminen oletusarvo).
    - Tallenna alilomakkeen päivitetyt arvot ja toimi seuraavasti:
        - Valitse vasemmasta ruudusta jaettu koodi ja alilomakkeita
        - Valitse uuden alilomakkeen, ja valitse rakenne - rakenne-ominaisuudet.
        - Valitse vasemmalla olevasta kolmannen-välilehti ja valitse **Propagate tämän kieltoja rakenteen muuttaminen**.
7. Avaa ${nimi} ExtensibleSchema alilomakkeen (jossa {nimi} on oletusarvoinen rakenteellisia objektiluokka, esimerkiksi – henkilön nimeä).
8. Lisää resurssi ja valitse alilomake (jonka olet luonut esimerkiksi – TestSubform) ja Tallenna ${nimi} ExtensibleSchema alilomakkeen.
9. Korvaa Domino Directory-malli {PUBNAMES. NTF} uuden mukautetun mallin {CONTOSO. NTF} seuraavat [näiden ohjeiden](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html)mukaan.
10. Domino järjestelmänvalvojan Sulje ja avaa Domino-konsolin LDAP-palvelun käynnistäminen uudelleen ja lataa LDAP-rakennetta:
    - Lisää Domino-konsolin **Domino-komennon** teksti tallennettu käynnistämään LDAP-palvelu - [Uudelleen tehtävän LDAP](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html)-komennosta.
    - Lataa LDAP rakenteen komennolla kertoa LDAP **Kertoa LDAP-ReloadSchema**.
11. Avaa Domino järjestelmänvalvoja ja valitse henkilöt ja ryhmät-välilehteä, jotta näet domino Lisää henkilö näkyy lisätty määritettä (muut-kohdassa välilehti).
12. Avaa Schema.nsf **tiedostot** -välilehti ja katso lisätty määrite näkyy TestSubform LDAP Aux objektin luokkaan.

**Menetelmä 3: Lisää mukautettu määrite ExtensibleObject-luokka**

1. Tiedoston avaaminen pääkansion sijoitettu {Schema.nsf}
2. Valitse LDAP objektiluokat vasemmasta valikosta **kaikki rakenne** -tiedostot ja valitse **Lisää objekti-luokan** painiketta:
3. Anna LDAP muodossa {zzzExtensibleSchema} (jossa zzz on oletusarvon rakenteellisia objektiluokka, kuten henkilön nimeä). Laajenna rakenne, henkilön objektiluokka, anna LDAP-nimi {PersonExtensibleSchema}.
4. Anna esimies objektin luokkanimi, jonka haluat laajentaa rakenne. Laajenna henkilön objektiluokan rakenteen, anna esimies objektin luokkanimi {dominoPerson}:
5. Anna kelvollinen OID, vastaava objektin-luokka.
6. Valitse laajennettu/mukautetut määritteet pakollinen tai valinnainen määritetyypit kentät tarpeen mukaan:
7. Kun olet lisännyt pakolliset määritteet ExtensibleObjectClass, valitse **Tallenna ja sulje**.
8. ExtensibleObjectClass luodaan vastaaviin oletusarvon objektiluokan Laajennetut määritteet.

## <a name="troubleshooting"></a>Vianmääritys

-   Saat lisätietoja siitä, miten voit ottaa kirjaamisen vianmääritys yhdistin, [Voit ottaa käyttöön tapahtumien seuranta jäljitys yhdistimet](http://go.microsoft.com/fwlink/?LinkId=335731).
