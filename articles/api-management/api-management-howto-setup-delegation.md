<properties 
    pageTitle="Voit siirtää käyttäjän rekisteröinti ja tuotteen tilauksen" 
    description="Opettele delegoida käyttäjän rekisteröinti ja tuotteen tilauksen kolmannelle osapuolelle Azure API hallinta." 
    services="api-management" 
    documentationCenter="" 
    authors="antonba" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="antonba"/>

# <a name="how-to-delegate-user-registration-and-product-subscription"></a>Voit siirtää käyttäjän rekisteröinti ja tuotteen tilauksen

Delegoinnin avulla voit käyttää nykyisen sivuston miten developer merkki-kohdassa/sign-korotus ja tilauksen tuotteet verrattuna sisäistä toimintoa käyttäen developer-portaalissa. Näin sivuston omista käyttäjätiedot ja tee näin vahvistamisen mukautetun tavalla.

## <a name="delegate-signin-up"> </a>Ryhmäkäytännön kehittäjä, kirjaudu sisään ja kirjautuminen

Määritettävän kehittäjä, kirjaudu sisään ja tarvitset erityistä delegointi päätepisteen luominen sivustoon, joka toimii pyynnön käynnistetty API hallinta developer-portaalissa, aloituskohtaa nykyisen sivuston rekisteröitymistoiminto.

Lopullinen työnkulku on seuraavasti:

1. Kirjaudu sisään tai kirjautumisen linkin API hallinta developer-portaalissa osoitteessa napsautuksella Developer
2. Selain ohjataan delegointi päätepiste
3. Delegointi päätepisteen samalla ohjaa tai esittää kysyy käyttäjän kirjautuminen tai ilmoittautuminen Käyttöliittymä
4. Onnistui, valitse käyttäjän ohjataan takaisin API hallinta developer portaalin sivulle ne käynnistetään


Aloita japanin ensimmäisen määritetään API hallinta reitittämään pyytää delegointi päätepisteen kautta. Valitse publisher API hallinta-portaalissa Valitse **Suojaus** ja valitse sitten **delegointi** -välilehti. Valitse 'Edustajan Kirjaudu sisään ja ilmoittautuminen' käyttöön-valintaruutu.

![Delegointi-sivu][api-management-delegation-signin-up]

* Määritä, mitä erityistä delegointi päätepisteen URL-osoite ja kirjoita **delegointi päätepisteen URL-osoite** -kenttään. 

* **Delegointi todennuksen avain** -kenttä lisää salaisuus, jota käytetään laskemiseen allekirjoituksen antamien varmistaaksesi, että pyyntö on varmasti peräisin Azure API hallinta vahvistusta varten. Voit valita **Luo** -painiketta, jolloin API Managemnet Luo avain satunnaisesti puolestasi.

Kun haluat luoda **delegointi päätepiste**. Se on tekemällä hakuun liittyviä toimintoja:

1. Pyyntö tulee seuraavassa muodossa:

    > *{URL-osoite, lähde-sivun} http://www.yourwebsite.com/apimdelegation?operation=SignIn&returnUrl= & suola = {merkkijono} & allekirjoitus = {merkkijono}*

    Kirjaudu sisään- / kirjautumisen kirjainkokoa kyselyparametrit:
    - **toiminto**: määrittää tyypin delegointi pyytää niiden on – **kirjautumisikkuna** voi olla vain tässä tapauksessa
    - **returnUrl**: sivun kohtaa, johon käyttäjä napsauttaa Kirjaudu sisään tai kirjautumisen linkin URL-osoite
    - **suola**: määräten suolan merkkijonon käytettäviä tietojenkäsittely suojauksen hash
    - **allekirjoitus**: laskettu suojaus-hajautuksen voidaan käyttää vertailuun omia luvut hash

2. Varmista, että pyyntö on peräisin Azure API Management (valinnainen, mutta erittäin suositeltava suojaus)

    * Laske HMAC SHA512-hash merkkijonon **returnUrl** ja **suola** kyselyparametrit ([esimerkkikoodi jäljempänä]) perusteella:
        > HMAC (**suolan** + "\n" + **returnUrl**)
         
    * Vertaa **allekirjoitus** kysely-parametrin arvon hajautuksen yläpuolella luvut. Jos kaksi hajautusarvot vastaavat, siirry seuraavaan vaiheeseen, muuten hylätä pyynnön.

2. Varmista, että saavat pyynnön merkki-ja merkki-ylöspäin: **toiminnon** kyselyn parametri määritetään "**kirjautumisikkuna**".

3. Käyttäjän esitellä Käyttöliittymän Kirjaudu sisään tai kirjautuminen

4. Jos käyttäjä on kirjautuminen ylöspäin sinun on luotava vastaavan tilin niiden API hallinnassa. [Luo] API hallinta REST-ohjelmointirajapinnalla. Kun teet näin, varmista Aseta Käyttäjätunnus sama käyttäjä Storessa on tai ID, jolla voit seurantaan, joka.

5. Kun käyttäjä todennetaan onnistui:

    * API hallinta REST-Ohjelmointirajapinnalla kautta [pyytää single-kertakirjautumisportaaliin (SSO) tunnus]

    * Liitä returnUrl kyselyparametri API-kutsu saamasi SSO URL-osoite:
        > esimerkiksi https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url 

    * käyttäjän ohjaaminen yllä valmistettu URL-osoite

Lisäksi **kirjautumisikkuna** -toiminto voidaan suorittaa myös tilinhallinta noudattamalla edellä kuvatut vaiheet ja käytä jotakin seuraavista toimista.

-   **Salasanan vaihtaminen**
-   **ChangeProfile**
-   **CloseAccount**

Sinun on välitettävä tilin hallintatoiminnot seuraavat kyselyparametrit.

-   **toiminto**: määrittää tyypin delegointi pyynnön (salasanan vaihtaminen, ChangeProfile tai CloseAccount)
-   **käyttäjätunnus**: hallinta-tilin käyttäjätunnus
-   **suola**: määräten suolan merkkijonon käytettäviä tietojenkäsittely suojauksen hash
-   **allekirjoitus**: laskettu suojaus-hajautuksen voidaan käyttää vertailuun omia luvut hash

## <a name="delegate-product-subscription"> </a>Delegoiminen tuotteen tilauksen

Tuotteen tilauksen delegoiminen toimii samalla tavalla voit delegoiminen käyttäjän kirjautuminen/ylöspäin. Lopullinen työnkulun olisi seuraavasti:

1. Kehittäjä valitsee tuotteen Ohjelmointirajapinnan hallinta developer-portaalissa ja napsauttaa tilaa-painiketta
2. Selain ohjataan delegointi päätepiste
3. Delegointi päätepisteen suorittaa tarvittavat tuotteen tilauksen vaiheet – tämä sinun ja se voi aiheuttaa uudelleenohjaus toiselle sivulle, voit pyytää laskutustiedot muut kysymisen tai yksinkertaisesti tietojasi ja eivät edellytä mitään käyttäjän toimia


Voit ottaa toiminnon käyttöön, valitse **edustajan tuotteen tilauksen** **delegointi** -sivulla.

Varmista, että delegointi päätepisteen tekee seuraavat toimet:


1. Pyyntö tulee seuraavassa muodossa:

    > *http://www.yourwebsite.com/apimdelegation?operation= {toiminnon} & tuotetunnus = {tuotteen tilata} & käyttäjätunnus = {käyttäjä pyynnön} & suolan = {merkkijono} & allekirjoitus = {merkkijono}*

    Tuotteen tilauksen kirjainkokoa kyselyparametrit:
    - **toiminto**: tunnistaa delegointi pyynnön tyyppi. Tuotteen tilauksen pyynnöt vaihtoehdot ovat:
        - "Tilaa": pyynnön tilaa tietyn tuotteen kanssa käyttäjän annettu tunnus (Katso alla oleva kuva)
        - "Tilauksen": pyynnön tuotteen käyttäjältä
        - "Uusi": requst uusi tilaus (kuten, joka voi olla vanhenee)
    - **tuotetunnus**: käyttäjä haluaa tilata tuotteen tunnus
    - **käyttäjätunnus**: käyttäjä, jolle pyyntö tehdään tunnus
    - **suola**: määräten suolan merkkijonon käytettäviä tietojenkäsittely suojauksen hash
    - **allekirjoitus**: laskettu suojaus-hajautuksen voidaan käyttää vertailuun omia luvut hash


2. Varmista, että pyyntö on peräisin Azure API Management (valinnainen, mutta erittäin suositeltava suojaus)

    * Laske HMAC-SHA512 merkkijonon **tuotetunnus**-, **käyttäjätunnus** - ja **suola** kyselyn parametrien perusteella:
        > HMAC (**suolan** + "\n" + **tuotetunnus** + "\n" + **käyttäjätunnus**)
         
    * Vertaa **allekirjoitus** kysely-parametrin arvon hajautuksen yläpuolella luvut. Jos kaksi hajautusarvot vastaavat, siirry seuraavaan vaiheeseen, muuten hylätä pyynnön.
    
3. Suorittaa **toiminnon** – esimerkiksi laskutus-edelleen kysymyksiä jne toiminto lajin tuotteen tilauksen käsittely.

4. Tilaamisen tuotteen että reunassa käyttäjän onnistuneesti, valitse tilaa API hallinta-tuotteen käyttäjän soittamalla [Tuotteen tilauksen REST-Ohjelmointirajapinnalla].

## <a name="delegate-example-code"></a> Esimerkkikoodi ##

Nämä MALLIKOODEJA näyttää, miten tulevat *delegointi vahvistus-näppäintä*, joka on määritetty delegointi näytön publisher-portaalin HMAC koon muuttamiseen tarkoitettu sitten allekirjoituksen vahvistaminen todistamalla välitetty returnUrl kelpoisuus luomiseen. Samaa koodia toimii tuotetunnus-ja käyttäjätunnus ja pieniä muutoksia.

**C#-koodin returnUrl Hajautuksen luominen**

    using System.Security.Cryptography;

    string key = "delegation validation key";
    string returnUrl = "returnUrl query parameter";
    string salt = "salt query parameter";
    string signature;
    using (var encoder = new HMACSHA512(Convert.FromBase64String(key)))
    {
        signature = Convert.ToBase64String(encoder.ComputeHash(Encoding.UTF8.GetBytes(salt + "\n" + returnUrl)));
        // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
        // compare signature to sig query parameter
    }


**NodeJS koodi returnUrl Hajautuksen luominen**

    var crypto = require('crypto');
    
    var key = 'delegation validation key'; 
    var returnUrl = 'returnUrl query parameter';
    var salt = 'salt query parameter';
    
    var hmac = crypto.createHmac('sha512', new Buffer(key, 'base64'));
    var digest = hmac.update(salt + '\n' + returnUrl).digest();
    // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
    // compare signature to sig query parameter
    
    var signature = digest.toString('base64');

## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja delegointi seuraavassa videossa.

> [AZURE.VIDEO delegating-user-authentication-and-product-subscription-to-a-3rd-party-site]

[Delegating developer sign-in and sign-up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
[Pyydä single-kertakirjautumisportaaliin (SSO) tunnus]: http://go.microsoft.com/fwlink/?LinkId=507409
[käyttäjän luominen]: http://go.microsoft.com/fwlink/?LinkId=507655#CreateUser
[soittavan tuotteen tilauksen REST-Ohjelmointirajapinnalla]: http://go.microsoft.com/fwlink/?LinkId=507655#SSO
[Next steps]: #next-steps
[jäljempänä esimerkkikoodi]: #delegate-example-code

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 