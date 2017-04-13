<properties
    pageTitle="Opi käyttämään käyttöoikeuksien hallinta (Java) | Microsoft Azure"
    description="Lue, miten kehittää ja käytöstä käyttöoikeuksien valvonta Java Azure-tietokannassa."
    services="active-directory" 
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm" />

# <a name="how-to-authenticate-web-users-with-azure-access-control-service-using-eclipse"></a>Miten verkko-käyttäjät, joilla on Azure Access Control-palvelu Pimennys tarkistamiseen

Tässä oppaassa kerrotaan, kuinka Azure Access ohjausobjektin Service (ACS) sisällä Azure-työkalujen käytettävät Pimennys. Lisätietoja ACS-kohdassa [seuraavat vaiheet](#next_steps) .

> [AZURE.NOTE]
> Azure Access Services ohjausobjektin suodatin on yhteisön tekniikka esikatselu. Kuin esiversion ei varsinaisesti tukee sitä Microsoft.

## <a name="what-is-acs"></a>Mikä on ACS?

Useimmat kehittäjät eivät ole tunnistetietojen asiantuntijoiden ja yleensä halua kuluvaa aikaa kehittämisestä todennuksen ja todennusmenetelmät niiden sovelluksia ja palveluja. ACS on Azure-palvelu, joka sisältää helposti, käyttäjät, joilla on pystyttävä käyttämään web-sovellusten ja palveluiden eikä sinun tarvitse kerroin monimutkaisia todennus logiikan oman koodiksi todennustapa.

Seuraavat ominaisuudet ovat käytettävissä ACS:

-   Integrointi Windows Identity Foundation (WIF).
-   Suositut web tunnistetietojen toimittajat (IP-osoitteet) mukaan lukien Windows Live ID-, Google-, Yahoo!- ja Facebook-tuki.
-   Active Directory-liittoutumispalvelut (AD FS) tuki 2.0.
-   Open Data Protocol (OData)-pohjainen management-palvelun, joka tarjoaa ohjelmallisesti ACS asetukset.
-   Hallinta-portaalin, joka sallii järjestelmänvalvojan oikeudet ACS-asetukset.

Saat lisätietoja ACS [Access ohjausobjektin palvelun 2.0][].

## <a name="concepts"></a>Käsitteitä

Azure ACS on muodostettu väitepohjaista identity - todennus-järjestelmiä sovellusten paikallisen tai pilveen luomiseen johdonmukaisen lähestymistavan ansaitun. Väitepohjaisten tunnistetietojen avulla yleisiä sovellusten ja palveluiden tunnistetietojen tarvitsemansa tiedot sisällä muiden organisaatioiden ja Internetissä niiden organisaation käyttäjien hankkia.

Tässä oppaassa tehtävien suorittamiseen pitäisi ymmärtää seuraavia käsitteitä:

**Asiakkaan** – tämä käyttöä koskevia artikkeleita kontekstissa tällä selainta, joka yrittää käyttää web-sovelluksen.

**Relying (RP) sovelluksen** - RP sovelluksen on sivuston tai palvelu, joka outsources yksi ulkoinen viranomainen todennusta. Käyttäjätietojen ammattikieli on ilmoittavat, RP luottaa viranomainen. Tässä oppaassa kerrotaan, miten voit määrittää sovelluksesi luottamus ACS.

**Suojaustunnuksen** - tunnus on kokoelma, joka on yleensä antaa käyttäjän todentaminen onnistuu suojaustietoja. Se sisältää *saatavat*, todennetun käyttäjän määritteet. Hakemuksen kuvaa käyttäjän nimi, tunnus-roolin käyttäjä kuuluu käyttäjän iän ja niin edelleen. Tunnus on yleensä digitaalisesti allekirjoitettu, mikä tarkoittaa sitä voidaan Orpotermit takaisin sen myöntäjä ja sen sisällöstä ei voi luvattomasti. Käyttäjä pääsee RP-sovellukseen, jos myöntäjä, RP sovelluksen luottaa kelvollinen tunnus.

**Tunnistetietojen toimittaja (IP)** - IP on myöntäjä, joka todentaa käyttäjätietojen ja suojauksen tunnusten ongelmat. Todellinen työmäärä varmenteiden tunnusten on toteutettu mutta määräten palvelu nimeltä suojauksen suojaustunnuksen Service (STS). Tyypillinen IP-osoitteet esimerkiksi Windows Live ID, Facebook, business käyttäjän säilöjen tietoihin (kuten Active Directory) ja niin edelleen.
Kun ACS on määritetty Salli IP, järjestelmä Hyväksy ja vahvista tunnusten kyseisen IP myöntämä. ACS luottaa useita IP-osoitteet samalla kertaa, mikä tarkoittaa, että kun sovelluksen luottaa ACS, voit välittömästi tarjota kaikki IP-osoitteet-kaikkien todennettujen käyttäjien sovelluksesi ACS luottaa puolestasi.

**Liitetty viestintä-palvelu (FP)** - IP-osoitteet suoraan tietävän käyttäjien ja todentaa ne käyttämällä tunnistetietoja ja antaa tietoja he tiedä niitä koskevia vaateita koskevat rajoitukset. Federation tarjoajan (FP) on myöntäjä muusta: sen sijaan, että todennustapa käyttäjät suoraan, se toimii edustaja ja vakuutuksenvälittäjien todennus välillä yhden RP ja yksi tai useampi IP-osoitteet. IP-osoitteet ja FPs antaa suojauksen tunnusten siksi ne käyttää suojauksen suojaustunnuksen Services (STS). ACS on yksi FP.

**ACS sääntö-ohjelma** - avulla saapuvat tunnusta luotettujen IP-osoitteet tunnusten tarkoitus kulutettu mukaan RP logiikan on kodifioitava yksinkertainen saatavat muunnos säännöt-lomakkeessa. ACS-välilehdessä sääntö-ohjelma, joka kestää varoen käyttöönottoa jostakin muunnos logiikan lisääminen RP määritetylle.

**ACS Namespace** - nimitila on ylimmän tason osion ACS, jota käytetään järjestämisessä asetuksia. Nimitilan pitää IP-osoitteet luotat, haluat yhteyshenkilönä RP sovellusten luettelo, että odotat sääntö säännöt moduuliin käsittelemään saapuvia tunnusten kanssa ja niin edelleen. Nimitilan paljastaa eri päätepisteet, jota käytetään sovelluksen ja kehittäjä saat ACS sen toiminto voidaan suorittaa.

Seuraavassa kuvassa näkyy, kuinka ACS todennus toimii web-sovelluksen:

![ACS Tietovuokaavion Esimerkki][acs_flow]

1.  (Kirjoita tässä tapauksessa selaimessa) asiakas pyytää sivun RP.
2.  Koska pyyntö ei ole vielä todennettu, RP ohjaa käyttäjän myöntäjä, se voi luottaa, joka on ACS. ACS näyttää käyttäjälle IP-osoitteet, joka on määritetty tämä RP valinta. Käyttäjä valitsee oikea IP.
3.  Asiakas IP-todennusta-sivu siirtyy ja käyttäjää kirjautumaan sisään.
4.  Kun asiakkaan todennetaan (esimerkiksi tunnistetiedot ovat kirjoittaa tunnistetietojen), IP-ongelmat suojaustunnus.
5.  Suojaustunnus antamista IP ohjaa asiakkaan ACS ja asiakas lähettää myöntämien IP ACS suojaustunnus.
6.  ACS tarkistaa IP-syötteiden käyttäjätietoja väittää-tunnuksessa kyselyjä ACS säännöt-ohjelma, laskee tulosteen tunnistetietojen saatavat ja ongelmien vianmääritys, joka sisältää näitä tulosteen väitteitä suojaustunnuksen myöntämä suojaustunnus.
7.  ACS ohjaa asiakas RP. Asiakastietokone lähettää myöntämien ACS RP suojaustunnuksen. RP tarkistaa ACS myöntämä suojaustunnus allekirjoitus, tarkistaa tunnuksessa saatavat ja palauttaa sivun tämän alun perin.

## <a name="prerequisites"></a>Edellytykset

Tässä oppaassa tehtävien suorittamiseen tarvitset seuraavat:

- Java Developer Kit (JDK), v 1,6 tai uudempi versio.
- Pimennys IDE Java ss kehittäjille Indigo tai uudempi versio. Tämä voi ladata <http://www.eclipse.org/downloads/>. 
- Java-pohjainen web-palvelimeen tai sovelluspalvelin, kuten Apache Tomcat, GlassFish, JBoss sovelluspalvelin tai jota jakauman.
- Azure tilauksen, joka on saatu <http://www.microsoft.com/windowsazure/offers/>.
- Azure-työkalujen Pimennys, saat huhtikuussa 2014 Vapauta tai uudempi versio. Lisätietoja on artikkelissa [asentamista varten Pimennys Azure-Työkalut](http://msdn.microsoft.com/library/windowsazure/hh690946.aspx).
- Sovelluksen kanssa käytettävät X.509-varmenne. Tarvitset tätä sertifikaattia julkisen varmenne (.cer) ja henkilökohtaisten tietojen vaihtaminen (. PFX)-muoto. (Tämän todistuksen luomiseen asetuksia on kuvaus jäljempänä tässä opetusohjelmassa).
- Azure tuntemusta Laske emulointikoneen ja käyttöönoton tekniikoita käsitellyt osoitteessa [luomalla Hei maailma sovellus Pimennys Azure varten](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx).

## <a name="create-an-acs-namespace"></a>Luo ACS Namespace

Kun haluat käyttää Access ohjausobjektin Service (ACS) Azure, sinun on luotava ACS nimitilan. Nimitilan on yksilöllinen vaikutusalueen osoitteen ACS resurssit-sovelluksessa.

1. Lokitiedoston [Azure hallinta-portaalin][].
2. Valitse **Active Directorysta**. 
3. Voit luoda uuden käyttöoikeuksien valvonta-nimitilan, **Uusi**, valitse **Sovelluksen palvelut**, valitse **Käytönvalvonta**ja valitse sitten **Nopea luominen**. 
4. Kirjoita nimi nimitilan. Azure varmistaa, että nimi on yksilöllinen.
5. Valitse alue, jossa nimitilan käytetään. Parhaan suorituskyvyn Käytä alue, johon otat sovelluksen.
6. Jos sinulla on useita tilauksia, valitse tilaus, jota haluat käyttää ACS nimitilan.
7. Valitse **Luo**.

Azure Luo ja aktivoi nimitilan. Odota, kunnes uusi nimitilan tila on **aktiivinen** , ennen kuin jatkat. 

## <a name="add-identity-providers"></a>Lisää tunnistetietojen toimittajat

Tässä tehtävässä voit lisätä IP-osoitteet todennuksessa RP-sovelluksen kanssa käytettäväksi. Esittelyä tämän tehtävän näyttää, miten voit lisätä Windows Live IP, mutta voit käyttää mitä tahansa ACS hallinta-portaalissa näkyvä IP-osoitteet.


1.  [Hallinta-portaalin Azure][] **Active Directory**ja valitse käytönvalvonta nimitilan **hallinta**. ACS hallinta-portaalin avautuu.
2.  Valitse vasemmassa siirtymisruudussa ACS hallinta-portaalin **tunnistetietojen toimittajat**.
3.  Windows Live ID-tunnus on oletusarvoisesti käytössä ja ei voi poistaa. Tässä opetusohjelmassa tarkoitetaan käytetään vain Windows Live ID. Tämä näyttö on kuitenkin kohtaa, johon voi lisätä muita IP-osoitteet napsauttamalla **Lisää** -painiketta.

Windows Live ID-tunnus on nyt käytössä ACS nimitilan IP nimellä. Seuraavaksi voit määrittää Java web-sovelluksen (avulla voi luoda myöhemmin) RP nimellä.

## <a name="add-a-relying-party-application"></a>Varmenteen käyttäjän osapuolen sovelluksen lisääminen

Tässä tehtävässä voit määrittää ACS tunnistettavan Java web-sovelluksen kelvollinen RP sovelluksena.

1.  Valitse ACS hallinta-portaalin **Relying osapuolien sovellukset**.
2.  Valitse **Käyttäisit osapuolien sovellukset** -sivulla **Lisää**.
3.  **Lisää käyttäisit osapuolen sovellus** -sivulla seuraavasti:
    1.  Kirjoita **nimi**RP nimi. Kirjoita tarkoitetaan tässä opetusohjelmassa **Azure Web Appissa**.
    2.  Valitse **tila**, **Määritä asetukset manuaalisesti**.
    3.  Kirjoita **alueen**, jota ACS myöntämä suojaustunnus koskee URI. Kirjoita tässä tehtävässä **8080 /**.
        ![Käyttäisit osapuolen alueen Laske emulaattorin käytettäviksi][relying_party_realm_emulator]
    4.  **Palauttaa URL** -osoitteen kirjoittamalla URL-osoite, johon ACS palauttaa suojauksen salasana. Kirjoita tässä tehtävässä **http://localhost:8080/MyACSHelloWorld/index.jsp**
        ![Relying osapuolen palauttaa URL-Osoitteen Laske emulaattorin käytettäviksi][relying_party_return_url_emulator]
    5.  Hyväksy oletusarvot loput kentät.

4.  Valitse **Tallenna**.

Olet nyt määrittänyt Java-web-sovelluksen kun se suoritetaan Azure Laske emulaattori (osoitteessa 8080 /) on RP ACS nimitilan. Seuraavaksi Luo säännöt, jotka ACS käyttää käsittelemiseen RP väitteitä.

## <a name="create-rules"></a>Sääntöjen luominen

Voit määrittää tämän tehtävän sääntöjä, joiden miten saatavat välitetään IP-osoitteet-kohdassa RP. Tämän oppaan varten on määritetään vain ACS kopioi syötteen varaa tiedostotyypit ja -arvojen ilman suodatusta tai muokata niitä suoraan-tulostus-tunnuksen.

1.  Valitse ACS hallinta-portaalin pääsivulta **säännön ryhmät**.
2.  Valitse **Sääntö ryhmät** -sivulla **Oletus säännön ryhmän Azure-verkkosovelluksessa**.
3.  **Muokkaa sääntöä-ryhmä** -sivun valitsemalla **Luo**.
4.  Valitse **Luo säännöt: Oletus säännön ryhmän Azure-verkkosovelluksessa** sivun, varmista, että Windows Live ID-tunnus on valittuna ja valitse sitten **Luo**.    
5.  **Muokkaa sääntöä-ryhmä** -sivun valitsemalla **Tallenna**.

## <a name="upload-a-certificate-to-your-acs-namespace"></a>Lataa varmenteen ACS nimitila

Tässä tehtävässä lataat. PFX-varmenne, jota käytetään Kirjaudu ACS nimitilan luoma suojaustunnuksen pyynnöt.

1.  Valitse ACS hallinta-portaalin pääsivulta **Varmenteet ja avaimet**.
2.  Valitse **Varmenteet ja avaimet** -sivulla **Lisää** yläpuolelle **Suojaustunnuksen allekirjoitus**.
3.  **Lisää tunnuksen allekirjoitusvarmenne tai avain** -sivulla:
    1. **Käytetään** -osassa **käyttäisit osapuolen** -sovellus ja valitse **Azure Web App** (jonka määrität aiemmin varmenteen käyttäjän osapuolen sovelluksen nimeä).
    2. Valitse **tyyppi** -osassa **X.509-varmenne**.
    3. Valitse **varmenne** -osassa napsauttamalla Selaa-painiketta ja siirry X.509-varmennetiedosto, jota haluat käyttää. Tämä on. PFX-tiedosto. Valitse tiedosto, valitse **Avaa**ja kirjoita sertifikaatin salasana **salasana** -ruutuun. Huomaa, että testausta varten voi käyttää Mykistä-signed-varmenne. Voit luoda itse allekirjoitettua varmennetta, (jäljempänä kuvattu) **ACS suodattimen kirjasto** -valintaikkunassa **Uusi** -painikkeella tai -apuohjelmalla **encutil.exe** Azure Starter Kit [projektin sivuston][] Java.
    4. Varmista, että **Tehdä ensisijainen** on valittuna. **Lisää tunnuksen allekirjoitusvarmenne tai avain** sivu pitäisi näyttää seuraavankaltaiselta.
        ![Lisää tunnuksen allekirjoitusvarmenne][add_token_signing_cert]
    5. Valitse Tallenna asetukset ja sulje **Lisää tunnuksen allekirjoitusvarmenne tai avainta** sivun **Tallenna** .

Seuraavaksi Tarkista sovelluksen integrointi-sivun tiedot ja kopioi, jotka haluat määrittää Java web-sovelluksen käyttämään ACS URI.

## <a name="review-the-application-integration-page"></a>Tarkista sovelluksen integrointi-sivulla

Löydät kaikki tiedot ja koodi määrittämiseksi Java-verkkosovelluksen (RP sovellus) ACS-sovelluksen integrointi sivulla ACS hallinta-portaalin käyttöä varten. Tarvitset näitä tietoja määritettäessä Java web-sovelluksen liitetyt todennusta varten.

1.  Valitse **sovelluksen integrointi**ACS hallinta-portaalin.  
2.  Valitse **Sovelluksen integrointi** -sivulla **Kirjautumissivu**.
3.  Valitse **Kirjaudu sisään sivun integrointi** -sivulla **Azure Web App**.

Valitse **Kirjautuminen sivun integrointi: Azure Web App** sivulla luetellut URL-osoite **vaihtoehto 1: linkki ACS isännöimä kirjautumissivulle** käytetään Java-web-sovelluksen. Tarvitset tätä arvoa silloin, kun Azure Access ohjausobjektin suodatin-kirjaston Java-sovellus.

## <a name="create-a-java-web-application"></a>Java-web-sovelluksen luominen
1. Sisällä Pimennys-valikosta **Tiedosto**, **Uusi**, ja valitse sitten **Dynaaminen Web-projekti**. (Jos et näe **Dynaaminen Web-projekti** peruutuksesta käytettävissä projektin jälkeen valitsemalla **Tiedosto**, **Uusi**, toimi seuraavasti: Valitse **Tiedosto**, **Uusi**, valitse **Projekti**, laajenna **Web**, valitse **Dynaaminen Web-projekti**ja valitse **Seuraava**.) Tässä opetusohjelmassa tarkoituksiin nimeä projektin **MyACSHelloWorld**. (Varmistaa nimeä, seuraavat vaiheet Tässä opetusohjelmassa toiminta SODAN tiedoston nimenä MyACSHelloWorld). Näyttöön tulee näkyviin seuraavankaltaiselta:

    ![Luo ACS exampple Hei maailma projektille][create_acs_hello_world]

    Valitse **Valmis**.
2. Laajenna **MyACSHelloWorld**Pimennys 's Project Explorer-näkymässä. **WebContent**hiiren kakkospainikkeella, valitse **Uusi**ja valitse sitten **JSP tiedosto**.
3. Nimeä tiedosto **index.jsp** **JSP uusi tiedosto** -valintaikkunassa. Ota pääkansion MyACSHelloWorld/WebContent kuin seuraavassa esitetyllä tavalla:

    ![ACS esimerkiksi JSP-tiedoston lisääminen][add_jsp_file_acs]

    Valitse **Seuraava**.

4. **Valitse JSP malli** -valintaikkunassa valitse **Uusi JSP-tiedosto (html)** ja valitse **Valmis**.
5. Kun index.jsp-tiedosto avautuu Pimennys, Lisää teksti **Hei ACS maailma!** sisällä nykyisen `<body>` elementti. Oman päivitetyt `<body>` sisältö tulee olla seuraavat:

        <body>
          <b><% out.println("Hello ACS World!"); %></b>
        </body>
    
    Tallenna index.jsp.
  
## <a name="add-the-acs-filter-library-to-your-application"></a>Lisää sovelluksen ACS suodatin-kirjasto

1. Pimennys 's Project Explorer- **MyACSHelloWorld**hiiren kakkospainikkeella, valitse **Muodosta polku**ja valitse sitten **Määritä luoda polku**.
2. **Java luominen polku** -valintaikkunassa valitse **kirjastot** -välilehti.
3. Valitse **Lisää kirjastoon**.
4. **Azure Access ohjausobjektin Services suodattamalla (MS Avaa Tech)** ja valitse sitten **Seuraava**. **Azure Access ohjausobjektin Services suodatin** -valintaikkuna tulee näkyviin.  ( **Sijainti** -kentässä voi olla eri polku, sen mukaan, johon asensit Pimennys, ja versionumero voi vaihdella sen mukaan, ohjelmistopäivitykset.)

    ![Lisää ACS suodatin-kirjasto][add_acs_filter_lib]

5. Selaimella avattu hallinta-portaalin **Kirjautuminen sivun integrointi** -sivulla kopioi luetellut URL-osoite **vaihtoehto 1: linkki ACS isännöimä kirjautumissivulle** kentän ja liitä se Pimennys-valintaikkunan **ACS todennus päätepiste** -kenttään.
6. Selaimella avattu hallinta-portaalin **Muokkaa käyttäisit valmistajan sovellus** -sivulle, kopioi **alueen** -kenttään URL-osoite ja liitä se Pimennys-valintaikkunan **Käyttäisit osapuolen alue** -kenttään.
7. Sisällä Pimennys-valintaikkunan **Suojaus** -osa, jos haluat käyttää varmenne, valitse **Selaa**, siirry haluamasi varmenne ja valitse **Avaa**. Tai jos haluat luoda uuden sertifikaatin, valitse **Uusi** , jos haluat näyttää **Uuden varmenne** -valintaikkuna, valitse Määritä salasana, .cer-tiedoston nimi ja uuden sertifikaatin .pfx-tiedoston nimi.
8. Valitse **Upota varmenteen SOTA-tiedostossa**. Varmenteen upottaminen tällä tavoin sisältää sen käyttöönoton tarvitsematta voi lisätä manuaalisesti osana. (Jos sen sijaan on tallennettava varmennetta ulkoisesti SOTA-tiedostosta, voit voi lisätä varmenteen roolin osana ja poista **Upota SODAN tiedoston varmenteen**.)
9. [Valinnainen] Säilytä **edellyttävät HTTPS yhteydet** valittuna. Jos tämä asetus on käytössä, ne on käyttää sovelluksen HTTPS-protokollan avulla. Jos et halua edellytä HTTPS-yhteyksiä, poista tämän valintaruudun valinta.
10. Käyttöönoton, Laske emulaattori **Azure ACS** suodatusasetukset näyttää seuraavankaltaiselta.

    ![Azure ACS-suodatusasetukset käyttöönottoa Laske emulaattori][add_acs_filter_lib_emulator]

11. Valitse **Valmis**.
12. Kun, valitse **Kyllä** valintaikkunan, jossa ilmoitetaan, että web.xml tiedosto luodaan ja näytetään.
13. Valitse **OK** ja sulje **Java luominen polku** -valintaikkuna.

## <a name="deploy-to-the-compute-emulator"></a>Laske-emulaattorin käyttöönottaminen

1. Pimennys 's Project Explorer- **MyACSHelloWorld**hiiren kakkospainikkeella ja valitse **Azure** **Azure pakkaaminen**.
2. **Projektin nimen**Kirjoita **MyAzureACSProject** ja valitse **Seuraava**.
3. Valitse JDK ja sovelluspalvelin. (Nämä vaiheet on kuvattu yksityiskohtaisesti [luodaan Hei maailma sovellusta varten Pimennys Azure](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) -opetusohjelma).
4. Valitse **Valmis**.
5. Napsauta **Azure emulaattorin Suorita** -painiketta.
6. Kun Java-web-sovelluksen käynnistänyt Laske emulaattori, Sulje kaikki esiintymät, selaimen (niin, että nykyiset selaimen-istunnot eivät häiritse ACS kirjautuminen-testi).
7. Suorita sovellus avaamalla <> 8080/MyACSHelloWorld/selaimessa ( <> tai/https://localhost:8080/MyACSHelloWorld/Jos **Vaadi HTTPS yhteydet**valittuna). Näyttöön tulee kehote Windows Live ID-kirjautumisen ja valitse sinun olisi otettava määritettyyn varmenteen käyttäjän osapuolen sovelluksen palauttaa URL-osoitteeseen.
99.  Kun olet tarkastellut sovelluksen, napsauta **Palauta Azure emulaattorin** -painiketta.

## <a name="deploy-to-azure"></a>Ottaa käyttöön Azure

Voit ottaa käyttöön Azure, sinun on varmenteen käyttäjän osapuolen alueen ja palauttaa URL-Osoitteen ACS nimitilan.

1. Azure-hallinta-portaalin **Muokkaa käyttäisit osapuolen sovellus** -sivulla Muokkaa **alueen** on otettu käyttöön sivuston URL-osoite. **Esimerkki** tilalle määrittämäsi käyttöönoton DNS-nimi.

    ![Käyttäisit osapuolen alueen tuotannon käytettäviksi][relying_party_realm_production]

2. Muokkaa **Palauttaa URL-osoite** on sovelluksen URL-osoite. **Esimerkki** tilalle määrittämäsi käyttöönoton DNS-nimi.

    ![Käyttäisit osapuolen palauttaa URL tuotannon käytettäviksi][relying_party_return_url_production]

3. Valitse Tallenna päivitetty vastaaminen osapuolen-alueen ja palauttaa URL-Osoitteen muutokset **Tallenna** .
4. Säilytä **Kirjautuminen sivun integrointi** sivun avattuna selaimessa, sinun on kopioi sen myöhemmin.
5. Pimennys 's Project Explorer- **MyACSHelloWorld**hiiren kakkospainikkeella, valitse **Muodosta polku**ja valitse sitten **Määritä luoda polku**.
6. Valitse **kirjastot** -välilehti, **Azure Access ohjausobjektin Services**suodatin ja valitse sitten **Muokkaa**.
7. Selaimella avattu hallinta-portaalin **Kirjautuminen sivun integrointi** -sivulla kopioi luetellut URL-osoite **vaihtoehto 1: linkki ACS isännöimä kirjautumissivulle** kentän ja liitä se Pimennys-valintaikkunan **ACS todennus päätepiste** -kenttään.
8. Selaimella avattu hallinta-portaalin **Muokkaa käyttäisit valmistajan sovellus** -sivulle, kopioi **alueen** -kenttään URL-osoite ja liitä se Pimennys-valintaikkunan **Käyttäisit osapuolen alue** -kenttään.
9. Sisällä Pimennys-valintaikkunan **Suojaus** -osa, jos haluat käyttää varmenne, valitse **Selaa**, siirry haluamasi varmenne ja valitse **Avaa**. Tai jos haluat luoda uuden sertifikaatin, valitse **Uusi** , jos haluat näyttää **Uuden varmenne** -valintaikkuna, valitse Määritä salasana, .cer-tiedoston nimi ja uuden sertifikaatin .pfx-tiedoston nimi.
10. Säilytä **Upota SODAN tiedoston varmenteen** valittuna, olettaen, että haluat upottaa varmenteen SOTA-tiedostossa.
11. [Valinnainen] Säilytä **edellyttävät HTTPS yhteydet** valittuna. Jos tämä asetus on käytössä, ne on käyttää sovelluksen HTTPS-protokollan avulla. Jos et halua edellytä HTTPS-yhteyksiä, poista tämän valintaruudun valinta.
12. Käyttöönoton Azure-Azure ACS suodatusasetukset näyttää seuraavankaltaiselta.

    ![Azure ACS-suodatusasetukset tuotannon käyttöönottoa varten][add_acs_filter_lib_production]

13. Valitse **Valmis** Sulje **Muokkaa kirjasto** -valintaikkuna.
14. Valitse **OK** ja sulje **MyACSHelloWorld ominaisuudet** -valintaikkunan.
15. Napsauta Pimennys, **Julkaise Azure pilveen** -painiketta. Vastaa näyttöön, samalla [luodaan Hei maailma sovellusta varten Pimennys Azure](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) -ohjeaiheen **ottamaan sovelluksesi Azure** -kohdan tehdyksi. 

Kun web-sovelluksen on otettu käyttöön, Sulje kaikki avoinna selaimessa-istunnot, web-sovelluksen käyttämiseen ja näyttöön tulee kehotus Kirjaudu sisään Windows Live ID-perässä on lähetetty varmenteen käyttäjän osapuolen sovelluksen palauttaa URL-osoite.

Kun olet tehnyt ACS Hei maailma-sovelluksesta, muista poistaa käyttöönoton (Lue, miten käyttöönoton [luodaan Hei maailma sovellusta varten Pimennys Azure](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) -kohdassa Poista).


## <a name="next_steps"></a>Seuraavat vaiheet

Tutustu tutkinnan, suojauksen vahvistus merkintä kieli (SAML) palauttama ACS sovelluksen [tarkastella SAML palauttama Azure Access Control-palvelu][]. Lisää Tutustu ACS's toiminnot ja kokeilla kehittyneempiä tilanteita, joissa on artikkelissa [Access ohjausobjektin palvelun 2.0][].

Tämä esimerkki käyttää myös **Upota SODAN tiedoston varmenne** -vaihtoehto. Tämän asetuksen avulla on helppo käyttöönotto varmenne. Jos haluat sen sijaan allekirjoitetun varmenteen erottaa SOTA-tiedostosta, voit käyttää seuraavia tekniikka:

1. **Azure Access ohjausobjektin suodatin** -valintaikkunan **Suojaus** -osassa Kirjoita $ **{kirjekuori. JAVA_HOME}/mycert.cer** ja poista **Upota varmenteen SOTA-tiedostossa**. (Muuta Jos sertifikaatin-tiedostonimi ei ole mycert.cer.) Valitse **Valmis** , sulje valintaikkuna.
2. Kopioi varmenteen käyttöönoton osana:-Pimennys 's Project Explorer Laajenna **MyAzureACSProject**, **WorkerRole1**hiiren kakkospainikkeella, valitse **Ominaisuudet**, laajenna **Azure rooli**ja valitse **osat**.
3. Valitse **Lisää**.
4. **Lisää osa** : valintaikkunan sisällä
    1. **Tuo** -osassa:
        1. **Tiedosto** -painikkeella voit siirtyä varmenne, jota haluat käyttää. 
        2. Valitse **Kopioi**- **menetelmää**.
    2. **Kuin nimi**Valitse tekstiruutu ja hyväksy oletusnimi.
    3. **Ota käyttöön** -osassa:
        1. Valitse **Kopioi**- **menetelmää**.
        2. Kirjoita **kansio**- **JAVA_HOME %**.
    4. **Lisää osan** -valintaikkunan pitäisi näyttää seuraavankaltaiselta.

        ![Varmenne-osan lisääminen][add_cert_component]

    5. Valitse **OK**.

Tässä vaiheessa varmennetta sisältyisivät käyttöönoton. Huomaa, että riippumatta siitä, onko varmenteen upottaminen SODAN tiedoston tai lisätä sen osana käyttöönoton, sinun on ladattava varmenteen lisääminen nimitilan [Lataa ACS nimitilan varmenne][] -kohdassa kuvatulla tavalla.

[What is ACS?]: #what-is
[Concepts]: #concepts
[Prerequisites]: #pre
[Create a Java web application]: #create-java-app
[Create an ACS Namespace]: #create-namespace
[Add Identity Providers]: #add-IP
[Add a Relying Party Application]: #add-RP
[Create Rules]: #create-rules
[Lataa varmenteen ACS nimitila]: #upload-certificate
[Review the Application Integration Page]: #review-app-int
[Configure Trust between ACS and Your ASP.NET Web Application]: #config-trust
[Add the ACS Filter library to your application]: #add_acs_filter_library
[Deploy to the compute emulator]: #deploy_compute_emulator
[Deploy to Azure]: #deploy_azure
[Next steps]: #next_steps
[Project-sivusto]: http://wastarterkit4java.codeplex.com/releases/view/61026
[Voit tarkastella SAML palauttama Azure Access Control-palvelu]: /en-us/develop/java/how-to-guides/view-saml-returned-by-acs/
[Access Control-palvelu 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[Windows Identity Foundation]: http://www.microsoft.com/download/en/details.aspx?id=17331
[Windows Identity Foundation SDK]: http://www.microsoft.com/download/en/details.aspx?id=4451
[Azure hallinta-portaalissa]: https://manage.windowsazure.com
[acs_flow]: ./media/active-directory-java-authenticate-users-access-control-eclipse/ACSFlow.png

<!-- Eclipse-specific -->
[add_acs_filter_lib]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibrary.png
[add_acs_filter_lib_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryEmulator.png
[add_acs_filter_lib_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryProduction.png

[relying_party_realm_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmEmulator.png
[relying_party_return_url_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLEmulator.png
[relying_party_realm_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmProduction.png
[relying_party_return_url_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLProduction.png
[add_cert_component]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddCertificateComponent.png
[add_jsp_file_acs]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddJSPFileACS.png
[create_acs_hello_world]: ./media/active-directory-java-authenticate-users-access-control-eclipse/CreateACSHelloWorld.png
[add_token_signing_cert]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddTokenSigningCertificate.png
 
