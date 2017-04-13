<properties
    pageTitle="Usean vuokraajan Web-sovelluksen kuvion | Microsoft Azure"
    description="Etsi arkkitehtuuri yhteenvetoja ja rakenne-kuviot, jotka kuvaavat ottamisesta käyttöön usean vuokraajan web-sovelluksen käyttöön Azure."
    services=""
    documentationCenter=".net"
    authors="wadepickett" 
    manager="wpickett"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/05/2015"
    ms.author="wpickett"/>

# <a name="multitenant-applications-in-azure"></a>Azure multitenant sovellukset

Multitenant sovelluksen on jaettu resurssi, joka sallii eri käyttäjä- tai "alihallintoihin," Näytä sovellus aivan kuin se oli omia. Tyypillinen tilanne, jossa kohdealueella multitenant sovelluksen on yksi jossa kaikki käyttäjät sovelluksen halutessasi käyttäjäkokemuksen mukauttamiseen, mutta on muuten samat liiketoimintaprosessien vaatimukset. Suuri multitenant sovellukset ovat Office 365, Outlook.com ja visualstudio.com.

Sovelluksen kehittäjän kannalta multitenancy edut liittyvät etupäässä toiminta- ja tehokkuus. Sovelluksen version voi tarpeiden useita alihallinnat/asiakkaita salliminen koonnin järjestelmän hallintatehtäviä kuten valvonta, suorituskyvyn säätö, ohjelmiston ylläpito ja tietojen varmuuskopioinnin.

Seuraavassa on luettelo tärkein tavoitteet ja vaatimukset palveluntarjoajan näkökulmasta.

- **Provisioning**: sinun on voitava valmistella uuden sovelluksen alihallinnat.  Multitenant sovellusten alihallinnat suuri määrä on yleensä tarvitse automatisoida tämän ottamalla Omatoiminen valmistelu.
- **Kunnossapidettävyys**: sinun on voitava sovelluksen ja muita ylläpitotehtäviä, kun useita alihallinnat käytön yhteydessä.
- **Seuranta**: sinun on voitava valvoa sovellusta aina mahdolliset ongelmat ja niistä voi etsiä virheitä. Tämä sisältää seuranta, miten kukin vuokraajan käyttää sovelluksen.

Oikein toteutettu multitenant sovelluksen sisältää seuraavat edut käyttäjille.

- **Eristystaso**: yksittäisiä alihallinnat toiminnot eivät vaikuta muiden omistajien mukaan sovelluksen käyttöä. Alihallinnat, jotka eivät voi käyttää muiden henkilöiden tiedot. Se näkyy vuokraajan aivan kuin heillä on yksinomaan sovellus.
- **Käytettävyys**: yksittäisiä alihallinnat, jotka haluat sovelluksen olisi jatkuvasti käytettävissä, esimerkiksi määritetty SLA oikeudet kanssa. Uudelleen muiden omistajien toiminnot eivät vaikuta sovelluksen käytettävyyttä.
- **Skaalattavuus**: sovellus Skaalaa täyttävän yksittäisiä alihallinnat tarvittaessa. Tavoitettavuus- ja muiden omistajien toiminnot eivät vaikuta suorituskykyyn sovelluksen.
- **Kustannusten**: kustannusten on pienempi kuin käynnissä erillinen, yksi vuokraajan sovelluksen, koska usean vuokraajan avulla resurssien jakaminen.
- **Customizability**. Mahdollisuus mukauttaa sovelluksen yksittäisiä vuokraajan, kuten lisäämällä tai poistamalla ominaisuudet, värit ja logon muuttaminen tai lisääminen myös omia koodia tai komentosarjaa monella tavalla.

Lyhyesti sanottuna on monta seikat, jotka on otettava huomioon erittäin skaalattava-palvelun tarjoamista varten, liittyy myös useita tavoitteet ja vaatimukset, jotka löytyvät monet multitenant sovellukset. Jotkin eivät välttämättä ole merkitystä tietyissä skenaarioissa ja tärkeys yksittäisiä tavoitteet ja vaatimukset vaihtelevat kunkin skenaariossa. Multitenant sovelluksen palveluntarjoaja, kun sinulla on myös tavoitteet ja vaatimukset, kokous alihallinnat tavoitteet ja vaatimukset, kannattavuutta, Laskutus, useita palvelutasot, valmistelu, kunnossapidettävyys seuranta ja Automaattiset.

Saat lisätietoja muita tyyliseikat multitenant sovelluksen [isännöinnin usean vuokraajan Azure-ohjelman][]. Yleisiä tietoja arkkitehtuuri kuviot usean vuokraajan ohjelmiston nimellä--palvelun (SaaS) tietokannan sovellusten tietoja on artikkelissa [Rakenne kuviot usean vuokraajan SaaS sovellusten Azure SQL-tietokanta](./sql-database/sql-database-design-patterns-multi-tenancy-saas-applications.md). 

Azure on useita toimintoja, jotka mahdollistavat avaimen ongelmia, kun suunnittelet multitenant järjestelmä havaitsi.

**Eristystaso**

- Osion sivuston omistajien Host otsikot kanssa tai ilman SSL viestintä
- Osion sivuston omistajien mukaan kyselyparametrit
- Työntekijän roolien verkkopalvelut
    - Työntekijän roolit. jotka yleensä käsitellä tietoja sovelluksen taustassa.
    - Web-roolit, joilla on yleensä edusta-sovellusten edustajana.

**Tallennustilan**

Tietojen hallinta, kuten olevan taulukon palvelun tarjoaa rakenteeton suurista tietomääristä tallennustilaan palvelut ja joka tarjoaa paljon rakenteeton tekstiä tallentamiseen Blob-palvelun Azure SQL-tietokanta tai Azuren tallennustilaan palveluihin tai binaaritietoja, kuten video, ääni- ja kuvia.

- Tietojen suojaamisesta Multitenant SQL-tietokantaan tarvittavat kohden-vuokraajan SQL Server-kirjautumiset.
- Käytät Azure-taulukoiden sovelluksen resurssien määrittämällä säilö käyttöoikeustaso käytännön, voit mahdollisuus mukauttaa eikä sinun tarvitse myöntää uusi URL-osoite on suojattu jaettua käyttöä allekirjoitukset resurssien käyttöoikeudet.
- Azure olevien sovellusten resurssien Azure olevien varten käytetään yleensä asema käsittely omistajien puolesta, mutta se voidaan myös jakaa tarvittavaa työmäärää valmistelussa tai hallinta.
- Palvelun Bus olevien sovellusten resurssien, joka vie toimivat ja jaetun palvelu, voit käyttää yhden jonon missä vuokraajan kullekin lähettäjälle vain oikeudet (Katso johdetaan väitteitä myöntämä ACS) siirtää kyseisen jonon, mutta vain vastaanottajia-palvelusta on oikeus hakea jonossa useita alihallinnat, jotka tulevat tiedot.


**Yhteys- ja tietoturva-palvelut**

- Service-Bus Azure-viestinvälityksen infrastruktuuri, joka on avattujen he sovellukset voivat vaihtaa viestejä löyhästi kytkettyjä tapa, jolla parannettu asteikko ja vikasietoisuudelle välillä.

**Verkkopalvelut**

Azure tarjoaa useita verkkopalvelut, jotka tukevat todennus ja parantaa isännöityä sovellusten hallinta. Nämä palvelut ovat seuraavat:

- Azure VPN avulla voit valmistella ja hallita näennäinen yksityisverkko (VPN) Azure sekä linkittää ne suojatusti paikalliseen IT-infrastruktuurin.
- Voit ladata saldo saapuvan liikenteen kaikkiin isännöityä Azure palveluihin, onko käytössäsi on sama palvelinkeskuksen tai useasta eri puolilla maailmaa palvelinkeskusten Virtual verkon liikenteen hallinta.
- Azure Active Directory (Azure AD) on Nykyaikainen ja REST-pohjainen palvelu, joka tarjoaa käyttäjätietojen hallinta ja käyttöoikeudet ohjausobjektin ominaisuuksia cloud-sovellukset. Azure AD-sovelluksen resurssien Azure AD voit antaa helposti käyttöoikeudet ja myöntää käyttäjille oikeuden käyttää web-sovellusten ja palveluiden todennus ominaisuuksia luvan monimenetelmäinen koodisi ulos samalla käytettäessä
- Azure palvelun Bus on suojattu viestintä ja tietojen kulun varten, jakaa ja hybrid sovellusten, kuten välisen Azure isännöidään sovellukset ja paikallisen sovelluksia ja palveluja, tarvitsematta monimutkaisia palomuurin ja suojaus-infrastruktuurin. Palvelun Bus välitys käyttämällä sovelluksen resurssien palvelut, jotka ovat paljastaa päätepisteet voivat kuulua vuokraajan (esimerkiksi tietueita ulkopuolella järjestelmä, kuten paikallinen) tai ne voivat olla services valmisteltu vuokraajan varten (koska luottamuksellisia vuokraajan kielikohtaiset tiedot kulkevan ne).



**Resurssien valmistelu**

Azure on usealla eri tavalla säännöstä uusi alihallinnat sovelluksen. Multitenant sovellusten alihallinnat suuri määrä on yleensä tarvitse automatisoida tämän ottamalla Omatoiminen valmistelu.

- Työntekijän roolien avulla voit säätää ja jään säännöstä kohden resurssit (esimerkiksi kun uuden vuokraajan etumerkki ylöspäin tai peruuttaa), kerätä arvot niin, että Käytä ja hallita seuraavan tiettyjä aikataulun asteikko tai vastauksena suorituskykyilmaisimet kynnysarvot kohdan itse. Tämä sama rooli voidaan myös painaa päivityksiä ja päivityksiä ratkaisuun.
- Azure-BLOB-objektit voidaan käyttää valmistelu Laske tai valmiiksi alustaa uuden alihallinnat käyttämisessä säilö käyttöoikeustaso käytännöt suojaaminen Laske tallennustilan resurssit palvelun pakettien Näennäiskiintolevyn kuvia ja muita resursseja.
- Vuokraajan valmistelu SQL-tietokantaan resurssien asetukset ovat:

    -   DDL komentosarjoissa tai upotetun resursseina kokoonpanot sisällä
    -   SQL Server 2008 R2 DAC paketit käyttöön ohjelmallisesti.
    -   Perusmuodon hakutietokanta kopioiminen
    -   Tietokannan käytön tuominen ja vieminen uusien tietokantojen valmistelu tiedostosta.



<!--links-->

[Usean vuokraajan Azure-ohjelman isännöinnin]: http://msdn.microsoft.com/library/hh534480.aspx
[Designing Multitenant Applications on Azure]: http://msdn.microsoft.com/library/windowsazure/hh689716
