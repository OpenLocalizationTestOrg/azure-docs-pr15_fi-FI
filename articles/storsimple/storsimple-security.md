<properties 
   pageTitle="StorSimple suojaus | Microsoft Azure" 
   description="Tässä artikkelissa kuvataan, suojaus ja tietosuoja-ominaisuuksia, jotka StorSimple-palvelu, laite sekä tiedot paikallisista ja pilvessä." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="05/03/2016"
   ms.author="v-sharos"/>

# <a name="storsimple-security-and-data-protection"></a>StorSimple suojaus ja tietojen suojaaminen

## <a name="overview"></a>Yleiskatsaus

Suojaus on suuria huolenaiheita kuka tahansa, jolla on antamista uusi tekniikka, erityisesti silloin, kun tekniikka käytetään luottamuksellisten tietojen tiedoilla. Kun arvioit eri tekniikoiden, ota huomioon parantavat riskit ja tietojen suojaaminen kustannuksiin. Microsoft Azure StorSimple tarjoaa suojaus ja tietosuoja-ratkaisun tietojen suojaaminen ja varmistaa: 

- **Luottamuksellisuutta** – vain valtuutettujen kohteiden voit tarkastella tietoja. 
- Vain valtuutetut kohteiden **eheys** – voit muokata tai poistaa tietoja.

Microsoft Azure StorSimple-ratkaisun kuuluu neljä pääosaa, olla vuorovaikutuksessa toistensa kanssa:

- **Microsoft Azure ylläpidettävä StorSimple hallintapalvelu** – hallintapalvelun, jonka avulla määrittää ja valmistella StorSimple laite.
- **StorSimple laitteen** – asennettu oman palvelinkeskuksen fyysinen laite. Isännät- ja asiakkaat, jotka luovat tietojen yhdistäminen StorSimple laitteen ja laitteen tietoja hallitaan ja siirtää sen Azure pilveen tarvittaessa.
- **Asiakkaiden/isännät yhteydessä laitteen** – infrastruktuurin asiakkaat, jotka StorSimple laitteen yhdistäminen ja luo tiedot, jotka on suojattu.
- **Pilvitallennustilaa** – sijainti, johon tiedot on tallennettu Azure pilveen.

Seuraavissa kohdissa kuvataan StorSimple suojausominaisuuksia, jotka auttavat suojaamaan kunkin nämä osat ja niiden tallennettuja tietoja. Se on myös luettelo kysymyksiä, jotka sinun on ehkä Microsoft Azure StorSimple suojaus ja vastaavat vastaukset.

## <a name="storsimple-manager-service-protection"></a>StorSimple hallinta palvelun suojaus

StorSimple hallintapalvelu on hallintapalvelun ylläpidettävä Microsoft Azure ja kaikki StorSimple-laitteet, joissa on hankittu organisaation hallinta. Voit käyttää StorSimple hallintapalvelu käyttämällä organisaation tunnistetietoja kirjautua Azure perinteinen portaalin selaimen kautta. 

Accessin StorSimple hallintapalveluun edellyttää, että organisaation Azure tilauksesta, joka sisältää StorSimple. Tilauksen ohjaa ominaisuudet, joita voit käyttää Azure perinteinen-portaalissa. Jos haluat lisätietoja niistä organisaatiossa ole Azure tilauksen, katso [Azure organisaationa rekisteröityminen](../active-directory/sign-up-organization.md). 

Azure-tietokannassa nykyisessä StorSimple hallintapalveluun, koska se on suojattu Azure suojaustoiminnot. Lisätietoja Microsoft Azure myöntämä suojaustoiminnot Siirry [Microsoft Azure-Valvontakeskus](https://azure.microsoft.com/support/trust-center/security/).

## <a name="storsimple-device-protection"></a>StorSimple laitteen suojaus

StorSimple laite on paikallisen-hybrid tallennustilan laitteen, jossa on tasainen tilan asemat (SSDs) ja kiintolevyaseman levyasemien (HDDs) ja tarpeettomat ohjaimet ja automaattinen automaattisesti ominaisuuksia. Tallennustilan tiering, markkinoille tällä hetkellä käytetyt (tai kuuma) paikallisen storage (-StorSimple laitteen tai paikalliseen palvelimet), siirrettäessä pilveen vähemmän usein käytettyjen tietojen hallinta ohjaimet.

Vain valtuutetut StorSimple laitteet voivat liittyä StorSimple hallintapalveluun, jonka loit Azure-tilaukseesi. Sallivat laitetta, sinun on rekisteröitävä se StorSimple hallinta-palvelussa antamalla palvelun rekisteröinti-näppäintä. Palvelun rekisteröinti avain on luotu Azure perinteinen portaalissa 128 bittistä satunnaisia avain. 

![Palvelun rekisteröinti avain](./media/storsimple-security/ServiceRegistrationKey.png)

Kerrotaan, miten poistaa palvelun rekisteröinti näppäintä, valitse [Vaihe 2: hankkia palvelun rekisteröinti avain](storsimple-deployment-walkthrough.md#step-2-get-the-service-registration-key).

Palvelun rekisteröinti on pitkä avainta, joka sisältää 100 +-merkkiä. Voit kopioida avain ja tallentaa sen tekstitiedostoon turvalliseen paikkaan, jolloin voit käyttää sitä sallivat muita laitteita tarpeen mukaan. Jos palvelun rekisteröinti avainta katoaa ensimmäisen laitteen rekisteröinnin jälkeen, voit luoda uuden tunnuksen StorSimple hallinta-palvelusta. Tämä ei vaikuta aiemmin laitteiden toiminnan. 

Kun laite on rekisteröity, se käyttää tunnusten Microsoft Azure yhteydessä. Palvelun rekisteröinti avainta ei käytetä laitteen rekisteröinnin jälkeen.

> [AZURE.NOTE] On suositeltavaa uudelleen palvelun rekisteröinti-näppäintä jokaisen käytön jälkeen.

## <a name="protect-your-storsimple-solution-via-passwords"></a>Suojaa StorSimple ratkaisu salasanat kautta

On tärkeää näkökohdat tietoturva ja käytetään yleisesti StorSimple-ratkaisussa voit varmistaa, että tiedot ovat käytettävissä vain valtuutettujen käyttäjien salasanan. StorSimple avulla voidaan määrittää seuraavat salasanat:

- StorSimple laitteen järjestelmänvalvojan salasana
- Haasta Handshake Authentication Protocol (CHAP) käynnistäjä ja kohde salasanat
- Salasanan StorSimple tilannevedoksen hallinta

### <a name="windows-powershell-for-storsimple-and-the-storsimple-device-administrator-password"></a>Windows PowerShellin StorSimple ja StorSimple laitteen järjestelmänvalvojan salasana

Windows PowerShell StorSimple on käyttöliittymä, joiden avulla voit hallita StorSimple laite. Windows PowerShell StorSimple on ominaisuuksia, jotka mahdollistavat Rekisteröi laite, Määritä verkkoliittymän laitteen, asenna päivitykset tietyntyyppisiä, vianmääritys laitteen muodostamalla yhteyden tuki-istunnon ja muuta laitteen tila. Voit käyttää Windows PowerShellin StorSimple yhdistämällä serial konsoliin laitteeseen tai käyttämällä Windows PowerShellin Etäpalvelujen.

PowerShellin Etäpalvelujen voidaan toteuttaa HTTPS tai HTTP. Jos etähallinnan HTTPS-protokollan välityksellä on käytössä, tarvitset etähallinnan laitteesta Lataa ja asenna se remote-asiakasohjelmassa. Lisätietoja PowerShell Etäpalvelujen Siirry [etäyhteyden muodostaminen StorSimple laitteen](storsimple-remote-connect.md).

Kun Windows PowerShell StorSimple avulla voit muodostaa yhteyden laitteeseen, sinun on antamaan laitteen järjestelmänvalvojan salasanan kirjautua laitteeseen.

![Laitteen järjestelmänvalvojan salasana](./media/storsimple-security/DeviceAdminPW.png)

Ota huomioon seuraavat parhaat käytännöt:

- Etähallinnan on oletusarvoisesti poistettu käytöstä. StorSimple hallinta-palvelun avulla voit ottaa sen käyttöön. Suojauksen käytäntönä etäkäyttöpalvelimen tulisi olla käytössä vain ajanjaksolla, joka todella tarvitaan.
- Jos olet muuttanut salasanan, muista ilmoittaa kaikki etäkäyttöpalvelimen käyttäjät niin, että ne ilmene odottamattomia connectivity tappio.
- StorSimple hallintapalvelu ei voi hakea aiemmin luotuja salasanoja: se palauttaa vain ne. On suositeltavaa, että tallennat kaikki salasanat turvallisessa paikassa niin, että sinulla ei ole salasanan, jos se on unohtunut. Jos tarvitset salasanan, muista ilmoittaa kaikille käyttäjille, ennen kuin voit nollata salasanasi. 

Voit käyttää Windows PowerShell-liittymän laitteeseen serial yhteyden avulla. Voit myös käyttää sitä etäyhteyden HTTP tai HTTPS, jotka tarjoavat lisäsuojauksen. HTTPS tarjoaa paremman suojauksen kuin sarja- tai HTTP-yhteyden. Kuitenkin käyttämään HTTPS on asennettava varmenteen asiakastietokoneen, joka käyttää laite. Voit ladata etäkäyttöpalvelimen varmenteen StorSimple hallintapalvelu laitteen määritys-sivulta. Jos etäkäyttöpalvelimen varmennetta katoaa, lataa uutta varmennetta ja välittäminen kaikille asiakkaille, joilla on oikeus käyttää etähallinnan.

### <a name="challenge-handshake-authentication-protocol-chap-initiator-and-target-passwords"></a>Haasta Handshake Authentication Protocol (CHAP) käynnistäjä ja kohde salasanat

CHAP on StorSimple laitteen käyttämät vahvistamaan remote asiakkaiden todennusmalli. Vahvistus perustuu jaetun salasana. CHAP voi olla yksisuuntainen (yksisuuntainen) tai keskinäiset (kaksisuuntainen). Kohde (StorSimple laite) todentaa Yksisuuntainen CHAP, jossa käynnistäjä (isäntä). Keskinäistä tai käänteisen CHAP edellyttää, että kohde todennetaan lähettäjä ja käynnistäjä todentaa kohde. Oman StorSimple voi määrittää joko menetelmää.

Kun määrität CHAP, otettava huomioon seuraavat:

- CHAP-käyttäjänimen on oltava alle 233 merkkiä.
- CHAP Salasanassa on oltava 12 – 16 merkkiä. Yrität käyttää pidempi käyttäjänimen tai salasanan aiheuttaa käyttöoikeuksien tarkistusvirhe Windows-isännän.
- Et voi käyttää saman salasanan CHAP lähettäjä ja CHAP kohde.
- Kun määrität salasanan, voi muuttaa, mutta ei voi palauttaa. Jos salasana on vaihdettu, muista ilmoittaa kaikki etäkäyttöpalvelimen käyttäjät niin, että he voivat muodostaa onnistuneesti StorSimple laite.

Lisätietoja CHAP ja StorSimple-ratkaisun määrittäminen Siirry [Määrittäminen CHAP StorSimple laitteeseesi](storsimple-configure-chap.md).

### <a name="storsimple-snapshot-manager-password"></a>Salasanan StorSimple tilannevedoksen hallinta

StorSimple tilannevedoksen hallinta on Microsoft Management Console (MMC)-laajennus, joka käyttää aseman ryhmät ja Windowsin tilannevedospalvelu sovelluksen yhdenmukaisia varmuuskopiot. Lisäksi voit luoda varmuuskopion aikatauluja ja Kloonaa tai palauttaa tietomääristä StorSimple tilannevedoksen hallinta.

Kun määrität laite, jota käytetään StorSimple tilannevedoksen hallinta, voit edellyttää annettava salasana StorSimple tilannevedoksen hallinta. Tätä ensimmäistä Aseta salasana Windows PowerShellin StorSimple rekisteröinnin yhteydessä. Salasanan myös voit määrittää ja muuttaa StorSimple hallinta-palvelusta. Salasana todentaa laitteen StorSimple tilannevedoksen hallinta.

![Salasanan StorSimple tilannevedoksen hallinta](./media/storsimple-security/SnapshotMgrPassword.png)

StorSimple tilannevedoksen hallinta salasanan on oltava 14 15 merkkiä ja 3 tai useita merkkejä isoja, pieni, numeeriset ja erikoismerkkejä yhdistelmän on oltava. Kun olet määrittänyt StorSimple tilannevedoksen hallinta-salasanan, voi muuttaa, mutta sitä ei voi hakea. Jos olet muuttanut salasanan, muista ilmoittaa kaikki etäkäyttäjien.

Lisätietoja StorSimple tilannevedoksen Managerin Siirry [Mikä on StorSimple tilannevedoksen hallinta?](storsimple-what-is-snapshot-manager.md)

### <a name="password-best-practices"></a>Salasanojen parhaista käytännöistä

On suositeltavaa, että varmistaa, että StorSimple salasanat ovat hyvä ja hyvin suojatun seuraavien ohjeiden avulla:

- Voit muuttaa salasanoja kolmen kuukauden välein. Salasanojen vaihtaminen pakotetaan vuosittain.
- Käytä vahva salasana. Jos haluat lisätietoja, valitse [Parempien salasanojen luominen ja suojaaminen lukitsemalla](http://blogs.microsoft.com/cybertrust/2014/08/25/create-stronger-passwords-and-protect-them/).
- Käytä aina eri salasanoja eri access-järjestelmiä; kunkin määrittämäsi salasana on oltava yksilöivä otsikko.
- Kuka tahansa, jolla ei ole oikeutta käyttää StorSimple-laitteen kanssa ei jaeta salasanat.
- Lue tietoja muiden eteen salasanan tai ei Vihje salasanan muoto.
- Jos epäilet, että tili tai salasana on ongelma, raportoi tietojen suojauksen osaston ajankohta.
- Käsittele kaikki salasanat luottamuksellisia luottamuksellisia tietoja. 

## <a name="storsimple-data-protection"></a>StorSimple tietojen suojaaminen

Tässä osassa kuvataan StorSimple suojausominaisuuksia, jotka suojaavat salataanko siirrettävät tiedot ja tallennettuja tietoja.

Muiden osien kuvatulla salasanoja käytetään sallivat ja todentaa käyttäjät, ennen kuin he voivat käyttää StorSimple ratkaisu. Toisen suojauksen huomiota on tietojen suojaaminen luvattomia käyttäjiä samalla, kun se siirretään tallennustilan järjestelmien ja samalla, kun se on tallennettu. Seuraavissa kohdissa kuvataan StorSimple mukana tietojen suojauksen ominaisuuksia.

> [AZURE.NOTE] Kopioinnin peruuttaminen tarjoaa lisäsuojauksen StorSimple laitteessa ja Microsoft Azuren tallennustilaan tallennettuja tietoja. Kun tiedot on deduplicated, tieto-objektit tallennetaan erikseen metatietojen määrittäminen ja käyttää niitä käytetään: kontekstia ei ole käytettävissä tallennuskiintiön varoitustason, uudistaa tietoja aseman rakennetta, tiedostojärjestelmässä ja tiedostonimi.

## <a name="protect-data-flowing-through-the-service"></a>Palvelun kautta juoksutus tietojen suojaaminen

StorSimple hallintapalvelu ensisijainen tarkoituksena on StorSimple-laitteen määrittäminen ja hallinta. Microsoft Azure suoritetaan StorSimple hallintapalvelu. Kirjoittaminen laitteen kokoonpanotietoja Azure perinteinen portaalin avulla ja sitten Microsoft Azure käyttää StorSimple hallintapalvelu Lähetä tiedot laitteeseen. StorSimple käyttää järjestelmän julkiseen avaimen paria varmistaa, että vaarantaminen Azure-palvelu ei tuota vaarantaminen tallennettuja tietoja. 

![Salauksen lennon](./media/storsimple-security/DataEncryption.png)

Julkiseen avaimen system auttaa suojaamaan tiedot, jotka kulkee palvelun seuraavasti:

1. Tietoja salausvarmenteen, joka käyttää on julkiseen julkisista ja yksityisistä avain pari laitteeseen luodaan ja käytetään tietojen suojaamiseen. Näppäimet luodaan, kun ensimmäinen laite on rekisteröity. 
2. Tietojen salausavaimet varmenteen viedään henkilökohtaisten tietojen vaihtaminen (.pfx)-tiedostoon, joka on suojattu palvelu tietojen salaus-näppäintä, joka on hyvä 128 bittistä-näppäintä, joka on ensimmäinen laite satunnaisesti luotu rekisteröinnin yhteydessä.
3. Julkinen avain varmenteen suojatusti saataville StorSimple hallintapalveluun ja yksityinen avain jää laitteen mukana.
4. Tietojen syöttäminen palvelu on salattu varmistaa, että Azure-palvelu ei voi poistaa juoksutus laitteen tiedot julkisen avaimen ja purettu käyttämällä laitteessa, yksityinen avain.

Palvelun tiedot salausavaimen luodaan vain ensimmäisen rekisteröity palveluun laitteeseen. Kaikki seuraavat laitteet, joka on rekisteröity-palvelun kanssa on käytettävä samaa tietojen salausavaimen. 

> [AZURE.IMPORTANT] 
> 
> On erittäin tärkeää palvelun tiedot salausavaimen kopioiminen ja tallenna se turvalliseen paikkaan. Palvelun tiedot salausavaimen kopio on tallennettu siten, että se valtuutettu henkilö voi käyttää, ja voit helposti tiedoksi laitteen järjestelmänvalvoja.
>
> Jos palvelun tiedot salausavain katoaa, Microsoft-tuen henkilön avulla voit hakea ne, jos käytössäsi on vähintään yksi laite online-tilaan. Suosittelemme, että palvelun tiedot salausavaimen muuttaa sen jälkeen, kun se on noudettu. Siirry ohjeet [vaihtaminen palvelun tiedot salausavaimen](storsimple-service-dashboard.md#change-the-service-data-encryption-key).

Voit muuttaa palvelun tiedot salausavaimen ja vastaavat tiedot salausvarmenne valitsemalla palvelun koontinäytössä **muuttaa palvelun tiedot salausavaimen** -vaihtoehto. Voit varmistaa, että tietojen suojauksen ei käsiin, täytyy muuttaa palvelun tiedot salausavaimen fyysistä StorSimple laitetta avulla. Muuttaminen salausavaimet edellyttää, että kaikki laitteet päivity uusi avain. Tästä syystä suosittelemme avain muuttuu, kun kaikki laitteet ovat online-tilassa. Jos offline-tilassa laitteita, niiden näppäimet voi muuttaa eri kerrallaan. Laitteet ja vanhentunut näppäimet on pysty suorittamaan varmuuskopioita, mutta he eivät voi palauttaa tietoja, ennen kuin avain on päivitetty. Lisätietoja on Katso [StorSimple hallinnan palvelun Raporttinäkymät-ikkunan](storsimple-service-dashboard.md).

Palvelun tiedot salausavaimen ja tietojen salausvarmenne ei vanhene. Suosittelemme kuitenkin, että muutat palvelun tiedot salausavaimen vuosittain, jos haluat estää Avainkompromissi.

## <a name="protect-data-at-rest"></a>Suojata muut tiedot

StorSimple laitteen hallitsee tiedot tallentamalla ne tasoa paikallisesti ja pilvipalvelussa, korkojakso käytön mukaan. Kaikissa Host (isäntä)-tietokoneissa, jotka ovat yhteydessä laitteen tietojen lähettäminen olevan laitteen tiedot siirtyy pilvipalveluun, tarpeen mukaan. Tiedot siirretään laitteesta pilveen suojatusti Internetin välityksellä. Kullakin laitteella on yksi iSCSI-kohteen, kaikki jaetun asemat pintoja kyseiseen laitteeseen. Kaikki tiedot salataan ennen sen lähettämistä cloud tallennustilan. 

![Cloud tallennustilan salausavain](./media/storsimple-security/CloudStorageEncryption.png)

Voit varmistaa suojaus ja siirtää pilveen tietojen eheyteen, StorSimple avulla voit määrittää cloud tallennustilan salausavaimet seuraavasti:

- Voit määrittää cloud tallennustilan salausavaimen aseman säilön luodessasi. Avain ei voi muokata tai lisätä myöhemmin. 
- Kaikki asemat aseman säilössä jakaa saman salausavaimen. Vaihtoehtoisen lomakkeen määritetyn aseman salauksen halutessasi Suosittelemme, että luot uuden aseman säilön isännöimiseen asemaan.
- Kun kirjoitat cloud tallennustilan salausavaimen StorSimple hallintapalveluun, avain salataan käyttämällä palvelun tiedot salausavaimen julkisen osan ja lähettää sitten laitteen.
- Cloud tallennustilan salausavaimen ei ole tallennettu missä tahansa palvelun ja tiedetään vain laite.
- Cloud-tallennustilan salausavaimen määrittäminen on valinnainen. Voit lähettää tiedot, jotka on salattu isännällä laitteeseen.

### <a name="additional-security-best-practices"></a>Lisäsuojauksen parhaat käytännöt

- Jaa liikenne: eristää oman iSCSI SAN käyttäjän liikenteen yrityksen lähiverkon käyttöönotto kokonaan erillisten verkon ja käyttämällä näennäislähiverkkojen jossa fyysinen eristystaso ei ole haluamasi vaihtoehto. Verkon iSCSI tallennuksessa taata turvallisuus ja yrityksen tärkeiden tietojen suorituskykyä. Sekoittamalla tallennustilasta ja käyttäjän liikenne yrityksen lähiverkossa ei ole suositeltavaa ja parantaa viive ja aiheuttaa verkon virheitä.

- Käytä isännän verkkosuojauksen verkkoliittymät, jotka tukevat TCP/IP purku ohjelma (ARVIOINTIKOHTEEN). ARVIOINTIKOHTEEN vähentää suorittimen kuormituksen TCP käsittelystä verkkosovittimen.

## <a name="protect-data-via-storage-accounts"></a>Tallennustilan tilit-tietojen suojaamisesta

Microsoft Azure jokaisen tilauksen voit luoda yhden tai useamman tallennustilan tilit. (Tallennustilan tilin sisältää yksilöllisen nimitilan Azure pilveen tallennettujen tietojen käsittelemisessä.) Tallennustilan tilin käyttöoikeus ohjataan tallennustilan tiliin liittyvien tilauksen ja access-näppäimiä. 

Kun luot tallennustilan tilin, Microsoft Azure Luo kaksi 512-bittinen tallennustilan pikanäppäimet, joista käytetään todennusta varten, kun StorSimple laitteen käyttää tallennustilan tilin. Huomaa, että vain yksi painettavat näppäimet on käytössä. Muut avain säilytetään varaa, joilla voit kiertää näppäimet säännöllisin väliajoin. Kun haluat kiertää näppäimet, aktivoitavan toissijaisen avaimen ja Poista perusavain. Voit luoda uuden tunnuksen käytettäväksi sitten seuraavan kiertämisen aikana. (Tietoturvasyistä monta palvelinkeskusten vaativat avaimen kierto.) 

Suosittelemme, että noudattamalla näitä parhaita käytäntöjä avaimen kierto:

- Tallennustilan tilin näppäimet säännöllisesti, jos haluat varmistaa, että tilisi tallennustilan ei käytä luvattomia käyttäjiä kannattaa Kierrä.
- Azure järjestelmänvalvojasi olisi määräajoin, muuttaminen tai uudelleen käyttämällä Azure perinteinen portaalin tallennustila-kohdasta tallennustilan tilin käyttöä suoraan ensisijaisen tai toissijaisen-näppäintä.


## <a name="protect-data-via-encryption"></a>Suojata tietoja kautta salaus

StorSimple käyttää seuraavia salausalgoritmeista, tallennettujen tietojen tai matkustaminen StorSimple-ratkaisun osien välillä.

| Algoritmin | Pituus | Protokollat/sovellukset/kommentit |
| --------- | ---------- | ------------------------------- |
| RSA       | 2 048       | RSA PKCS 1 v1.5 käytetään Azure perinteinen portaalin salaamaan määritystietoja, jotka lähetetään laitteen: esimerkiksi tallennustilan tilin tunnistetietojen StorSimple laitteen määrittäminen ja cloud tallennustilan salausavaimet. |
| AES       | 256        | Salaa palvelun tiedot salausavaimen julkisen osan ennen sen lähettämistä Azure perinteinen-portaaliin StorSimple laitteesta käytetään AES CBC kanssa. Sitä käytetään myös StorSimple laitteen salaamaan tiedot, ennen kuin tiedot lähetetään cloud-tallennustilan tilin. |


## <a name="storsimple-virtual-device-security"></a>StorSimple virtual laitteen suojaus

[AZURE.INCLUDE [storsimple virtual device security](../../includes/storsimple-virtual-device-security.md)]

## <a name="frequently-asked-questions-faq"></a>Usein kysytyt kysymykset

Seuraavassa on joitakin liittyviä kysymyksiä ja vastauksia suojaus- ja Microsoft Azure StorSimple.

**Q:** Oma palvelu on ongelmia. Mikä on oltava oma seuraavat vaiheet?

**A:** Sinun on muutettava heti palvelun tiedot salausavaimen ja tallennustilaa tili, jota käytetään tiering tietojen tallennustilan tilin avaimet. Ohjeet Siirry osoitteeseen: 

- [Palvelun tiedot salausavaimen muuttaminen](storsimple-service-dashboard.md#change-the-service-data-encryption-key)
- [Tallennustilan tilien avaimen kierto](storsimple-manage-storage-accounts.md#key-rotation-of-storage-accounts)

**Q:** Minulla on uusi StorSimple laitteella, joka pyytää palvelun rekisteröinti-näppäintä. Miten voin hakea ne?

**A:** Tämä avain on luotu StorSimple hallintapalvelu luomisen yhteydessä. Kun muodostat yhteyden laitteeseen StorSimple hallintapalveluun, tarkastelemaan tai palvelun rekisteröinti avain uudelleen sivussa palvelun pika-aloitusopas. Luodaan uusi palvelu rekisteröinti avain ei vaikuta aiemmin rekisteröidyn laitteet. Ohjeet Siirry osoitteeseen:

- [Tarkastele tai palvelun rekisteröinti avain uudelleen](storsimple-service-dashboard.md#view-or-regenerate-the-service-registration-key)

**Q:** Olen kadottanut palvelun Omat tiedot salausavaimen. Mitä teen?

**A:** Ota yhteys Microsoftin tuotetukeen. Ne voit kirjautua sisään tuki-istunnon laite ja ohjeita avaimen hakemiseen (jos vähintään yksi laite on verkossa). Heti, kun olet hankkinut palvelun tiedot salausavaimen, muuta se varmistaaksesi, että uusi avain kutsutaan voi käyttää. Ohjeet Siirry osoitteeseen:

- [Palvelun tiedot salausavaimen muuttaminen](storsimple-service-dashboard.md#change-the-service-data-encryption-key)

**Q:**  Käytän valtuutettuja laitteen palvelun tiedot salauksen avaimen muutoksen, mutta ei voitu käynnistää avaimen muutos-prosessin. Mitä minun pitäisi tehdä?

**A:** Jos aikakatkaisuajan on päättynyt, sinun on reauthorize palvelun tietojen salauksen avaimen muuttaminen laitteen ja aloita uudelleen.

**Q:**  Palvelun tiedot salausavain on muutettu, mutta minulla ei voi päivittää muiden laitteiden enintään neljän tunnin kuluessa. Tarvitsen käynnistää uudelleen?

**A:** 4 tunnin ajanjakso on tarkoitettu vain aloitetaan muutos. Kun aloitat päivitysprosessin valtuutettuja StorSimple laitteeseen, luvan kelpaa, ennen kuin kaikki laitteet on päivitetty.

**Q:** Tutustu StorSimple-järjestelmänvalvoja on poistunut yrityksen. Mitä minun pitäisi tehdä?

**A:** Muuttaa salasanojen vaihtaminen, joka mahdollistaa StorSimple-laitteen käyttö ja muuttaa palvelun tiedot salausavaimen varmistaa, että uudet tiedot ei tiedetä luvattoman henkilökunnan. Ohjeet Siirry osoitteeseen:

- [Storsimple salasanojen StorSimple hallinta-palvelun avulla](storsimple-change-passwords.md)
- [Palvelun tiedot salausavaimen muuttaminen](storsimple-service-dashboard.md#change-the-service-data-encryption-key)
- [Määritä CHAP StorSimple laitteeseesi](storsimple-configure-chap.md)

**Q:** Haluan antaa StorSimple tilannevedoksen hallinta salasanan isäntään, joka muodostaa yhteyden StorSimple laitteeseen, mutta salasanaa ei ole käytettävissä. Mitä voin tehdä?

**A:** Jos unohdat salasanan, sinun on luotava uusi. Valitse muista kaikki aiemmin luodut käyttäjät ilmoittaa, että salasana on vaihdettu ja että ne on päivitettävä niiden asiakkaiden uudella salasanalla. Ohjeet Siirry osoitteeseen:

- [StorSimple tilannevedoksen hallinta salasanan vaihtaminen](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password)
- [Todennusta käyttävän laitteen](storsimple-snapshot-manager-manage-devices.md#authenticate-a-device)

**Q:** Laitteessa on muutettu StorSimple etäkäyttöpalvelimen Windows PowerShell varmennetta. Kuinka päivitän etäkäyttöpalvelimen asiakkaat?

**A:** Voit ladata uutta varmennetta StorSimple hallinta-palvelusta ja annettava se on asennettu sertifikaatin kaupan etäkäyttöpalvelimen asiakkaita. Ohjeet Siirry osoitteeseen:

- [Tuo varmenne cmdlet-komento](https://technet.microsoft.com/library/hh848630.aspx)

**Q:** On Omat tiedot suojattuja, jos palvelun käsiin StorSimple esimies?

**A:** Palvelun määritysten tiedot salataan julkisella avaimella aina, kun tarkastelet selaimessa. Koska palvelu ei ole käyttöoikeutta yksityinen avain-palvelu ei näy mitään tietoja. Jos StorSimple hallintapalvelu on ongelmia, ei ole vaikutusta ei ei ole tallennettu StorSimple hallintapalvelu avaimet.

**Q:** Jos joku pääsee käyttämään tietoja salausvarmenteen, tiedot voidaan käsiin?

**A:** Microsoft Azure tallentaa asiakkaan tietoja salausavaimen (.pfx-tiedosto) salattuja muodossa. Koska .pfx-tiedosto on salattu ja StorSimple-palvelu ei ole tiedot salausavaimen salauksen .pfx-tiedosto, käytön yksinkertaisesti pääsy .pfx-tiedosto ei näytä mitään tietoja.

**Q:** Mitä tapahtuu, jos paikalliset kohteen pyytää Microsoft tietoni?

**A:** Koska kaikki tiedot salataan-palvelusta ja yksityinen avain on käytettävissä laitteen mukana, paikalliset kohde on pyydettävä asiakkaan tietoja. 

## <a name="next-steps"></a>Seuraavat vaiheet

[Ota käyttöön StorSimple laitteen](storsimple-deployment-walkthrough.md).
 
