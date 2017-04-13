<properties 
    pageTitle="Yhteystietojen integrointi Azure Monimenetelmäisen todentamisen ja Active Directoryn välillä"
    description="Tämä on Azure multi-factor authentication sivu, jossa kuvataan, miten voit integroida Azure multi-factor Authentication Server Active Directory-hakemistosta, jotta voit synkronoida kansiot."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="directory-integration-between-azure-mfa-server-and-active-directory"></a>Yhteystietojen integrointi Azure MFA Server ja Active Directoryn välillä

Yhteystietojen integrointi-osan avulla voit määrittää palvelimen integroida Active Directory- tai toinen LDAP-hakemisto.  Sen avulla voit määrittää vastaamaan directory-rakenne ja automaattinen synkronointi käyttäjien määrittäminen määritteet.

## <a name="settings"></a>Asetukset
Oletusarvon mukaan Azure multi-factor Authentication palvelin on määritetty tuotavan tai Synkronoi käyttäjät Active Directorysta.  Välilehden avulla voit ohittaa oletusasetukset ja sitominen eri LDAP-hakemiston, ADAM hakemiston tai tietyn Active Directory-toimialueen ohjauskoneen.  Sisältää myös LDAP-todennuksen LDAP-välityspalvelimen käyttöä tai LDAP sitoa RADIUS kohteeksi, vanhat todennus IIS-todennuksen tai käyttäjän portaalin ensisijainen käyttöoikeuden.  Seuraavassa taulukossa on kuvattu yksittäisiä asetuksia.

![Asetukset](./media/multi-factor-authentication-get-started-server-dirint/dirint.png)

| Toiminto | Kuvaus |
| ------- | ----------- |
| Active Directoryn avulla | Valitse Käytä Active Directory-asetus, kun Active Directory tuominen ja synkronointi.  Tämä on oletusarvo. <br>Huomautus: Tietokone on liitetty toimialueeseen ja sinun on kirjauduttava toimialueen tilillä Active Directory-integrointi toimii oikein. |
| Sisällytä luotettujen toimialueiden | Tarkista ovat luotettuja toimialueita-valintaruutu, jos haluat agentti yrität muodostaa domains on luotettu nykyisen toimialueen, toinen toimialue metsää tai toimialueiden osallistuvat metsää luota.  Ei viedessäsi tai käyttäjien miltä tahansa luotettujen toimialueiden synkronoiminen, poista valintaruudun valinta suorituskyvyn parantamiseksi.  Oletusarvo on valittuna. |
| Tietyn LDAP-määritys | Valitse Käytä LDAP-asetus, kun tuominen ja synkronointi määritettyjä LDAP-asetuksia. Huomautus: Käytä LDAP valittaessa käyttöliittymän muutoksista viittaukset Active Directorysta LDAP. |
| Muokkaa-painike | Muokkaa-painikkeen avulla muokattu nykyiset LDAP määritys-asetukset. |
| Määritteen laajuus kyselyjä | Ilmaisee, käytetäänkö määrite laajuus kyselyt.  Määritteen laajuus kyselyjen Salli tehokas directory hakujen ehdot täyttävät tietueet merkinnät toiseen tietueeseen määritteiden perusteella.  Azure multi-factor Authentication Server käyttää määrite laajuus kyselyjen kyselyn tehokkaasti käyttäjät, jotka ovat suojausryhmän jäsen.   <br>Huomautus: On joissakin tapauksissa, jossa määrite laajuus kyselyt ovat tuettuja, mutta ei kannata käyttää.  Esimerkiksi Active Directory voi olla ongelmat määrite laajuus kyselyt, kun käyttöoikeusryhmän sisältää useamman kuin yhden toimialueen jäseniä.  Tässä tapauksessa valintaruudun on oltava ole valittuna. |

Seuraavassa taulukossa on kuvattu LDAP-määrityksiä.

| Toiminto | Kuvaus |
| ------- | ----------- |
| Palvelin | Kirjoita isäntänimi tai LDAP-hakemisto-palvelimen IP-osoite.  Varmuuskopion palvelimen voidaan määrittää myös erotettu toisistaan puolipisteellä. <br>Huomautus: Kun sitoa tyyppi on SSL-täydellinen isäntänimi on yleensä pakollinen. |
| Base DN | Kirjoita peruskansio objekti, josta kaikki kansion kyselyt alkavat DN-nimi.  Jos esimerkiksi ohjauskoneen = abc, ohjauskoneen = com. |
| Sido tyyppi - kyselyt | Valitse haluamasi sitominen käytettäväksi muodostukseen LDAP-hakemistosta.  Tätä käytetään tuontia, synkronointi ja käyttäjänimi tarkkuus. <br><br>  Anonyymi - anonyymi sitominen suoritetaan.  Sidonta DN ja sitoa salasana ei käytetä.  Tämä versio toimii vain, jos LDAP-hakemiston avulla anonyymi sidonta ja käyttöoikeudet Salli tarvittavat tietueet ja määritteiden kyselyt.  <br><br> Yksinkertainen - sitominen DN ja sitoa salasana välitetään pelkkänä tekstinä, jos haluat sitoa LDAP-hakemisto.  Tämä käytetään vain testausta varten ja varmista, että palvelin tavoittaa ja sitominen käyttäjätilillä on tarvittavat oikeudet.  On suositeltavaa, että SSL käytetään sen sijaan, kun haluamasi varmenne on asennettu.  <br><br> SSL - sitominen DN ja sitoa salasana salataan SSL-yhteys sitoa LDAP-hakemisto.  Tämä edellyttää, että varmenne on asennettu paikallisesti LDAP-hakemiston luottaa.  <br><br> Windows - sitominen käyttäjänimen ja salasanan sitoa käytetään muodostamaan yhteyden Active Directory-toimialueen ohjauskoneen tai ADAM directory suojatusti.  Jos sidot käyttäjänimi on tyhjä,-on käyttäjätiliä käytetään sitoa. |
| Sido tyyppi - välityspalvelimia | Valitse haluamasi sitominen käytettäväksi LDAP sitominen todennuksessa.  Katso sidonta, kirjoita kuvaukset sitominen tyyppi - kyselyt-kohdassa.  Esimerkiksi näin anonyymi sitominen kyselyjen voidaan käyttää, kun SSL sitominen käytetään suojaamiseen LDAP sitominen välityspalvelimia. |
| Sidonta DN tai sitominen käyttäjänimi | Kirjoita LDAP-hakemiston muodostukseen käytettävän tilin käyttäjätietueen DN-nimi.<br><br>Sidonta DN-nimi käytetään vain, kun sitoa tyyppi on yksinkertainen tai SSL.  <br><br>Kirjoita Windows-tili, jota käytetään, kun sitominen LDAP-hakemiston, kun sitoa tyyppi on Windows-käyttäjänimesi.  Jos tyhjä,-on käyttäjätiliä käytetään sitoa. |
| Sido salasana | Kirjoita salasana sitominen sitoa DN tai käyttäjänimi käytetään sitoa LDAP-hakemisto.  Määrittämiseksi multi-factor Auth Server AdSync-palvelun salasanan synkronointi on otettava käyttöön ja sen on oltava käynnissä paikallisessa tietokoneessa.  Salasana tallennetaan tilissä multi-factor Auth Server AdSync-palvelu on käynnissä Windows tallennetut käyttäjänimet ja salasanat.  Salasana tallennetaan myös multi-factor Auth Server-käyttöliittymä toimii tili ja valitse tili, Multi-factor Auth Server-palvelu on käynnissä.  <br><br> Huomautus: Salasana on tallennettu vain paikallisen palvelimen Windows tallennetut käyttäjänimet ja salasanat, koska tämä vaihe on valmis kunkin multi-factor Auth palvelimeen, joka on pääsy salasana. |
| Kyselyn kokorajoitus | Määritä käyttäjät, joilla Hakemistohaku palauttavat enimmäismäärä kokorajoitus.  Tämä rajoitus on vastattava LDAP-hakemisto-määritys.  Suuri hakujen missä sivutus ei tueta tuominen ja synkronointi yritetään noutaa käyttäjien erissä.  Jos kokorajoitus määritetty tässä on suurempi kuin määritetty LDAP-hakemisto-rajoitus, osa käyttäjistä saattaa vastattu. |
| Testaa-painike | Napsauta Testaa LDAP-palvelimen sitominen testi-painiketta.  <br><br> Huomautus: Käytä LDAP-vaihtoehto ei tarvitse valittava halutaan testaaminen sidonta.  Näin sidonta testataan ennen käyttämällä LDAP-määritys. |

## <a name="filters"></a>Suodattimet
Suodattimien avulla voit määrittää ehdot, joiden oikeutettu tietueet Hakemistohaku suoritettaessa.  Suodattimen määrittämällä voit rajoittaa objektit, jonka haluat synkronoida.  

![Suodattimet](./media/multi-factor-authentication-get-started-server-dirint/dirint2.png)

Azure Monimenetelmäisen todentamisen on 3 seuraavista vaihtoehdoista:

- **Säilön suodatin** - suodattimen ehdot oikeutettu säilö tietueiden suoritettaessa directory-haun avulla.  Active Directory-ja ADAM (| () objectClass=organizationalUnit)(objectClass=container)) käytetään yleensä.  LDAP muihin kansioihin, suodatusehdot, joka saa kunkin säilö objektin laji on tarkoitus käyttää directory-rakenteen mukaan.  <br>Huomautus: Jos tyhjä, ((objectClass=organizationalUnit)(objectClass=container)) käytetään oletusarvon mukaan.

- **Suojaus-ryhmässä suodatin** - suodattimen ehdot oikeutettu suojauksen ryhmitellä tietueita suoritettaessa directory-haun avulla.  Active Directory-ja ADAM (&(objectCategory=group) (groupType:1.2.840.113556.1.4.804:=-2147483648)) käytetään yleensä.  LDAP muihin kansioihin, suodatusehdot, joka saa kunkin suojauksen ryhmän objektin tyyppi on tarkoitus käyttää directory-rakenteen mukaan.  <br>Huomautus: Jos tyhjä, (&(objectCategory=group) (groupType:1.2.840.113556.1.4.804:=-2147483648)) käytetään oletusarvon mukaan.

- **Käyttäjäsuodatin** - suodattimen ehdot oikeutettu käyttäjätietueita suoritettaessa directory-haun avulla.  Active Directory-ja ADAM, (ja (objectClass=user)(objectCategory=person)) yleensä avulla.  Muut LDAP-kansioissa (objectClass = inetOrgHenkilö) tai kaltaisen käytetään directory-rakenteen mukaan. <br>Huomautus: Jos tyhjä, (ja (objectCategory=person)(objectClass=user)) käytetään oletusarvon mukaan.

## <a name="attributes"></a>Määritteet
Määritteet voi mukauttaa tarpeen tietyn kansion.  Näin voit lisätä mukautetut määritteet ja hienosäätää määritteitä, jotka tarvitset synkronointi.  Määritteen kunkin kentän arvon pitäisi olla määritteen määritysten määritelty kansion nimi.  Käytä alla olevassa taulukossa lisätiedot.

![Määritteet](./media/multi-factor-authentication-get-started-server-dirint/dirint3.png)

| Toiminto | Kuvaus |
| ------- | ----------- |
| Yksilöllinen tunnus | Kirjoita määritteen nimi-määritteen, joka on yksilöllinen tunnus säilö, käyttöoikeusryhmän ja käyttäjätietueita.  Active Directoryssa tämä on yleensä objectGUID.  Muut LDAP-käyttöotot voi olla entryUUID tai kaltaisen.  Oletusarvo on objectGUID. |
| ... (Valitse määrite)-painikkeet | Määritteen jokainen kenttä on "..."-painike vieressä, joka näyttää salliminen määrite on valittuna luettelosta valitse määrite-valintaikkuna. <br><br>Valitse määrite-valintaikkuna.<br><br>Huomautus: Määritteet voidaan kirjoittaa manuaalisesti ja ei tarvitse vastaa määritteen määrite luettelosta. |
| Yksilöllinen tunnus-tyyppi | Yksilöllinen tunnus-määrite tyypin valitseminen  Active Directoryn objectGUID-määrite on GUID-tunnus-tyypin.  Muut LDAP-käyttöotot voi olla ASCII-tavuinen matriisi tai merkkijono.  Oletusarvo on GUID-tunnus. <br><br>Huomautus: On tärkeää määrittää tämän oikein, koska synkronoinnin kohteet, joihin viitataan niiden yksilöllinen ja yksilöllinen tunnus-tyypin käytetään Etsi suoraan objektin hakemistosta.  Asetukset Tämä merkkijono, kun hakemiston tallentaa arvon todella, kuten DBCS-matriisin ASCII-merkkien estää synkronoinnin toimimasta oikein. |
| DN-nimi | Kirjoita määritteen nimi, joka sisältää kunkin tietueen DN-nimi-määritteen.  Active Directoryssa tämä on yleensä distinguishedName.  Muut LDAP-käyttöotot voi olla entryDN tai kaltaisen.  Oletusarvo on distinguishedName. <br><br>Huomautus: Jos määritteen, joka sisältää vain DN-nimi ei ole, adspath-määrite voidaan.  "LDAP: / /<server>/" polku poistettava automaattisesti käytöstä, jätä vain DN objektin. |
| Säilön nimi | Kirjoita säilö tietueen nimen sisältävä määrite määritteen nimi.  Tämän määritteen arvo näkyvät hierarkiassa säilö tuotaessa Active Directorysta synkronoinnin kohteiden lisääminen tai.  Oletusarvo on nimi. <br><br>Huomautus: Jos eri säilöjen käyttää eri määritteet heidän nimensä, useita säilön nimi määritteitä voi erikseen puolipisteillä.  Löytyi säilö aluetta ensimmäisen säilö name-määritettä käytetään sen näyttönimi. |
| Suojauksen ryhmänimi | Kirjoita tietueen suojauksen ryhmän nimen sisältävä määrite määritteen nimi.  Tämän määritteen arvo näytetään käyttöoikeusryhmän luettelossa tuotaessa Active Directorysta synkronoinnin kohteiden lisääminen tai.  Oletusarvo on nimi. |
| Käyttäjät | Seuraavien määritteiden käytetään hakeminen näyttäminen, tuominen ja synkronoiminen käyttäjätiedot hakemiston. |
| Käyttäjänimi | Kirjoita määritteen nimi, joka sisältää tietueen käyttäjän käyttäjänimi-määritteen.  Tämän määritteen arvon käytetään multi-factor Auth palvelimen käyttäjänimi.  Toinen määrite voidaan määrittää ensimmäiselle varmuuskopioina.  Toinen määrite käytetään vain, jos ensimmäisen määritteen ei sisällä arvoa käyttäjälle.  Oletusarvot ovat userPrincipalName ja sAMAccountName. |
| Etunimi | Kirjoita määritteen nimi, joka sisältää tietueen käyttäjän etunimi-määritteen.  Oletusarvo on givenName. |
| Sukunimi | Kirjoita määritteen nimi, joka sisältää käyttäjätietueen Sukunimi-määritteen.  Oletusarvo on sn. |
| Sähköpostiosoite | Kirjoita määritteen nimi, joka sisältää tietueen käyttäjän sähköpostiosoite-määritteen.  Sähköpostiosoitetta käytetään voidaan lähettää Tervetuloa ja päivittää sähköpostit käyttäjälle.  Oletusarvo on sähköposti. |
| Käyttäjäryhmän | Kirjoita määritteen nimi, joka sisältää käyttäjän tietueen käyttäjäryhmä-määritteen.  Käyttäjäryhmän avulla voidaan suodattaa käyttäjät agentti ja raporteissa multi-factor Auth palvelimen hallinta-portaalissa. |
| Kuvaus | Kirjoita määritteen nimi, joka sisältää käyttäjän tietueen kuvaus-määritteen.  Kuvaus käytetään vain etsimistä varten.  Oletusarvo on kuvaus. |
| Vastaajan puhelun kieli | Kirjoita määritteen nimi, joka sisältää käyttäjän äänipuheluissa käytettävä kieli lyhyt nimi-määritteen. |
| SMS tekstin kieli | Kirjoita määritteen nimi, joka sisältää käyttäjän tekstiviestien käytettävä kieli lyhyt nimi-määritteen. |
| Puhelimen sovelluksen kieli | Kirjoita määritteen nimi, joka sisältää phone app tekstiviestejä käyttäjän käytettävä kieli lyhyt nimi-määritteen. |
| SIJASTA suojaustunnuksen kieli | Kirjoita määritteen nimi, joka sisältää kieli sijasta suojaustunnuksen tekstiviestien käyttäjän lyhyt nimi-määritteen. |
| Puhelimet | Tuominen ja synkronoiminen puhelinnumeroiden käytetään määritteet.  Jos määritteen nimi ei ole määritetty puhelimen tyyppi-puhelimen tyyppi eivät ole käytettävissä kun tuot Active Directorysta tai synkronoinnin kohteiden lisääminen. |
| Business | Kirjoita määritteen nimi, joka sisältää käyttäjän tietueen Työpuhelinnumero-määritteen.  Oletusarvo on telephoneNumber. |
| Aloitus | Kirjoita määritteen nimi, joka sisältää käyttäjän tietueen kotipuhelinnumeron määrite.  Oletusarvo on Kotipuhelin. |
| Hakulaite | Kirjoita määritteen nimi, joka sisältää käyttäjätietueen hakulaite luku-määritteen.  Oletusarvo on hakulaite. |
| Matkapuhelin | Kirjoita määritteen nimi, joka sisältää käyttäjätietueen matkapuhelinnumero-määritteen.  Oletusarvo on mobiili. |
| Faksin | Kirjoita määritteen nimi, joka sisältää käyttäjätietueen faksinumero määrite.  Oletusarvo on facsimileTelephoneNumber. |
| IP-puhelin | Kirjoita määritteen nimi, joka sisältää käyttäjän tietueen IP-puhelinnumero-määritteen.  Oletusarvo on ipPhone. |
| Mukautettu | Määritteen nimi, joka sisältää mukautetun puhelinnumero-määrite |
|  | käyttäjätietue.  Oletusarvo on tyhjä. |
| Tiedostotunniste | Kirjoita määritteen nimi, joka sisältää käyttäjän tietueen numero alanumero määrite.  Tunniste-kentän arvon käytetään vain Ensisijainen puhelinnumero laajennus.  Oletusarvo on tyhjä. <br><br>Huomautus: Jos tunniste-määritettä ei ole määritetty, tunnisteet voidaan sisällyttää osana puhelin-määrite.  "X" eteen tunnisteen, jotta se voi jäsentää.  Esimerkiksi 555-123-4567 x890 johtaa 555-123-4567 kuin puhelinnumero ja 890 kuin tunniste. |
| Palauta oletusarvot-painike | Palauttaa kaikki määritteet oletusarvoon valitsemalla Palauta oletusarvot.  Oletusarvoiset pitäisi toimia oikein Normaali Active Directory- tai ADAM rakenteen kanssa. |

Jos haluat muokata määritteitä, valitse määritteet-välilehden Muokkaa-painiketta.  Tämä tuo Windowsin, jonka avulla voit muokata määritteet.

![Muokkaa määritteet](./media/multi-factor-authentication-get-started-server-dirint/dirint4.png)

## <a name="synchronization"></a>Synkronointi
Synkronoinnin pitää Azure multi-factor käyttäjätietokannan synkronoida Active Directoryssa tai toinen Lightweight Directory Access Protocol (LDAP) Lightweight Directory Access Protocol-hakemisto-käyttäjien kanssa.  Prosessi on samanlainen kuin tuominen käyttäjät manuaalisesti Active Directorysta, mutta tekee säännöllisesti kyselyn Active Directory-käyttäjän ja suojauksen rooliryhmän muutosten käsittelemään.  Se sisältää myös poistaminen käytöstä tai poistamalla käyttäjiä säilö tai suojaus-ryhmä poistetaan ja poistamalla käyttäjiä poistaa Active Directorysta.

Multi-factor Auth ADSync-palvelu on Windows-palvelun, joka suorittaa Active Directory säännöllisiä kysely.  Tämä ei voi sekoittaa Azure AD-synkronointi tai Azure AD Connect.  Multi-factor Auth ADSync, mutta rakennettu samalla koodi-tietokannassa, on tietyn Azure multi-factor Authentication-palvelimeen.  Se on asennettu pysäytetty-tilaan ja aloitetaan multi-factor Auth Server-palvelu, kun määritetty suorittamaan.  Jos sinulla on usean palvelimen multi-factor Auth palvelinkokoonpanon, Multi-factor Auth ADSync voidaan suorittaa vain yhteen palvelimeen.

Multi-factor Auth ADSync-palvelu käyttää Microsoftin DirSync LDAP-palvelimen tunniste lisääminen tehokkaasti muutokset.  Tämä DirSync-ohjausobjektin soittajan on oltava "hakemisto Hae muutokset" oikealle ja DS-replikoinnin-Get-muutokset laajennettu käytönvalvonta oikealle.  Oletusarvon mukaan nämä käyttöoikeudet määritetään toimialueen ohjauskoneen järjestelmänvalvoja ja LocalSystem-tileihin.  Multi-factor Auth AdSync-palvelu on määritetty suorittamaan järjestelmänä oletusarvoisesti.  Tämän vuoksi on helpoin toimialueen ohjauskoneen palvelun suorittamiseen.  Palvelun toimivat tiliksi suppeampaan oikeuksilla, jos määrität suoritettava täyden käyttäjätietojen aina.  Tämä on yhtä tehokasta, mutta vaatii vähemmän tilin oikeudet.

Jos määritetty käyttämään LDAP- ja LDAP-hakemiston tukee DirSync-ohjausobjekti, sitten kysely käyttäjä-ja tietoturva rooliryhmän muutosten toimivat sama kuin se tekee Active Directory.  Jos LDAP-hakemiston ei tue DirSync-ohjausobjekti, täyden käyttäjätietojen suoritetaan kunkin jakson aikana.

![Synkronointi](./media/multi-factor-authentication-get-started-server-dirint/dirint5.png)

Käytä alla olevassa taulukossa Saat lisätietoja kunkin synkronointiasetusten yksittäisiä asetuksia.

| Toiminto | Kuvaus |
| ------- | ----------- |
| Active Directory-synkronoinnin ottaminen käyttöön | Kun valintaruutu on valittuna, Multi-factor Auth Server-palvelu käynnistetään lisääminen säännöllisesti Active Directory muutokset. <br><br>Huomautus: vähintään yksi synkronoinnin kohde on lisättävä ja Synkronoi nyt on suoritettava ennen multi-factor Auth Server-palvelu käynnistyy käsittelyn muutokset. |
| Synkronoi kaikki | Määritä aikaväli multi-factor Auth Server-palvelu odottaa kyselyt ja käsittelyn muutokset välillä. <br><br> Huomautus: Määritetyllä aikavälillä on kunkin jakson alkuun välisen ajan.  Jos ylittää käsittelyn muutokset ajan väli-palvelun kyselyn uudelleen heti. |
| Käyttäjien poistaminen enää Active Directoryssa | Kun valintaruutu on valittuna, Multi-factor Auth Server-palvelu käsittelee poistaa Active Directory-käyttäjän poistetuksi ja poistaa liittyvät multi-factor Auth palvelimen käyttäjän. |
| Suorita aina täydellinen synkronointi | Kun valintaruutu on valittuna, Multi-factor Auth Server-palvelu aina suorittaa täyden käyttäjätietojen.  Kun tarkistamatta, Multi-factor Auth Server-palvelu suorittaa lisäävän synkronoinnin tekemällä kyselyn vain käyttäjät, jotka ovat muuttuneet.  Oletusarvo on poistettu. <br><br> Huomautus: Kun tarkistamatta, lisäävän synkronoinnin voidaan suorittaa vain jos hakemiston tukee DirSync-ohjausobjektin ja sitoa hakemiston käytetty tili on DirSync vaiheittainen kyselyjen suorittamiseen tarvittavat käyttöoikeudet.  Jos tiliä ei ole tarvittavia oikeuksia tai synkronoinnin sisältyy useita toimialueita, Suorita täysi synkronointi-versiota. |
| Edellyttää järjestelmänvalvojan hyväksyntää, kun X useita käyttäjiä käytöstä tai poistettu | Synkronoinnin kohteita voi määrittää käytöstä tai poistaa käyttäjät, jotka eivät ole enää kohteen säilö tai käyttöoikeusryhmän jäsen.  Suojaavat, kuin järjestelmänvalvojan hyväksyntää voi olla tarvittavat kun käyttäjämäärä käytöstä tai poista kynnysarvo.  Kun valintaruutu on valittuna, hyväksynnän tarvitaan määritetyn raja-arvon.  Alue on 1-999 oletusarvo on 5. <br><br> Hyväksynnän helpottaa lähettämällä ensimmäisen sähköposti-ilmoituksen järjestelmänvalvojat. Sähköposti-ilmoituksen antaa ohjeet tarkistaminen ja hyväksyminen käytöstä ja käyttäjien poistaminen.  Kun multi-factor Auth Server-käyttöliittymä käynnistetään, se pyytää hyväksyntää. |

**Synkronoi heti** -painikkeen avulla voit suorittaa synkronoinnin kohteiden määritetty täydellinen synkronointi.  Täyden käyttäjätietojen tarvitaan aina, kun synkronointi kohteiden lisätty, muokata, poistaa tai tilattava.  Kannattaa myös tarvittavat, ennen kuin multi-factor Auth AdSync-palvelua voi toiminnallisia, koska se asettaa aloituskohdan, josta palvelu äänestyksen järjestäminen vaiheittainen muutokset.  Jos täydellinen synkronointi ei ole tehty synkronoinnin kohteet on tehty muutoksia, sinun tulee kehotus Synkronoi nyt siirryttäessä toiseen osaan tai käyttöliittymän suljettaessa.

**Poista** -painikkeen avulla voit poistaa yhden tai useamman kohteen synkronointi multi-factor Auth palvelimen synkronoinnin kohde-luettelosta järjestelmänvalvoja.

>[AZURE.WARNING]Kun synkronointi kohteen tietue on poistettu, sitä ei voi palauttaa. Tarvitset lisää synkronointi kohteen tietue Jos poistit sen vahingossa.

Synkronointi kohteen tai synkronoinnin kohteita on poistettu multi-factor Auth palvelimesta.  Multi-factor Auth Server-palvelu ei enää käsittelee synkronointi-kohteita.

Siirrä ylös- ja Siirrä alas-painikkeiden avulla järjestelmänvalvojat voivat muuttaa synkronoinnin kohteiden järjestystä.  Tilaus on tärkeää, koska sama käyttäjä voi olla useita synkronoinnin kohteita (kuten säilön ja käyttöoikeusryhmän) jäsen.  Käytetty synkronoinnissa käyttäjän asetukset tulevat, johon käyttäjä liittyy luettelon ensimmäiseen synkronoinnin kohteeseen.  Synkronoinnin kohteet tulisi vuoksi sijoittaa ensisijaisuusjärjestys.

>[AZURE.TIP]Täyden käyttäjätietojen pitäisi tehdä synkronoinnin kohteiden poistamisen jälkeen.  Täyden käyttäjätietojen suoritetaan jälkeen järjestys synkronoitavat kohteet.  Napsauta suorittamiseen täyden käyttäjätietojen Synkronoi heti-painiketta.

## <a name="multi-factor-auth-servers"></a>Multi-factor Auth palvelimet
Muut multi-factor Auth palvelimet voidaan määrittäminen tukemaan varmuuskopion SÄDE-välityspalvelimen, LDAP-välityspalvelimen tai IIS-todennusta varten. Synkronointi-määritykset jaetaan kaikkien niistä kesken. Vain yksi näistä tekijöiden voi kuitenkin olla multi-factor Auth Server-palvelun käytön. Tässä välilehdessä voit valita multi-factor Auth Server, pitäisi olla käytössä synkronointi.

![Avainhankkeiden monivaiheisen-Factor-Auth palvelimet](./media/multi-factor-authentication-get-started-server-dirint/dirint6.png)
