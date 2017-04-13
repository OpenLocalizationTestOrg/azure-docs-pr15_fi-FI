<properties 
    pageTitle="Ominaisuuksien käyttämisestä Azure API hallintakäytäntöjen tuki" 
    description="Lue, miten Azure API hallintakäytännöt ominaisuuksien avulla." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>


# <a name="how-to-use-properties-in-azure-api-management-policies"></a>Ominaisuuksien käyttämisestä Azure API hallintakäytäntöjen tuki

API hallintakäytännöt ovat tehokas ominaisuuksien järjestelmän, joka mahdollistaa Publisherin määrittämisen jälkeen Ohjelmointirajapinnan toiminnan muuttaminen. Käytännöt ovat lausekkeita, jotka suoritetaan järjestyksessä pyynnön kokoelma tai Ohjelmointirajapinnan vastaus. Käytännön väittämistä on rakennettava literaaliteksti arvoja, käytäntö lausekkeita ja ominaisuudet. 

Kunkin API hallinta esiintymää on ominaisuudet-kokoelmaan avain/arvo paria, jotka ovat yleisiä palveluesiintymä. Näiden ominaisuuksien avulla voidaan vakion merkkijonoarvoa hallitsemaan kaikki API määritys ja käytännöistä. Kullekin ominaisuudelle on määritteet.


| Määrite | Tyyppi            | Kuvaus                                                                                             |
|-----------|-----------------|---------------------------------------------------------------------------------------------------------|
| Nimi      | merkkijono          | Ominaisuuden nimi. Se sisältää vain kirjaimia, numeroita, ajan, viiva ja alaviivoja. |
| Arvo     | merkkijono          | Ominaisuuden arvo. Se saa olla tyhjiä tai koostua ainoastaan välilyöntejä.                           |
| Salainen    | Totuusarvo         | Määrittää, onko arvo salaisuus ja salattu vai ei.                                |
| Tunnisteet      | matriisin merkkijono. | Valinnainen tunnisteita, kun avulla voidaan suodattaa ominaisuus-luettelossa.                               |

Ominaisuudet määritetään **Ominaisuudet** -välilehdessä publisher-portaalissa. Seuraavassa esimerkissä on määritetty kolme ominaisuudet.

![Ominaisuudet:][api-management-properties]

Ominaisuusarvojen voi olla literaalimerkkijonot ja [käytännön lausekkeet](https://msdn.microsoft.com/library/azure/dn910913.aspx). Seuraavassa taulukossa on kolme edellisen ominaisuudet ja niiden määritteitä. Arvon `ExpressionProperty` on käytännön lauseke, joka palauttaa merkkijonon, joka sisältää päivämäärän ja kellonajan. Ominaisuuden `ContosoHeaderValue` on merkitty salainen, jotta sen arvo ei ole näkyvissä.

| Nimi               | Arvo                      | Salainen | Tunnisteet    |
|--------------------|----------------------------|--------|---------|
| ContosoHeader      | TrackingId                 | EPÄTOSI  | Contoso |
| ContosoHeaderValue | ••••••••••••••••••••••     | TOSI   | Contoso |
| ExpressionProperty | @(DateTime.Now.ToString()) | EPÄTOSI  |         |

## <a name="to-use-a-property"></a>Jos haluat käyttää ominaisuutta

Käyttää ominaisuutta käytännön, aseta ominaisuuden nimi sisällä aaltosulkeet, kaksinkertainen kahdet `{{ContosoHeader}}`, kuten seuraavassa esimerkissä.

    <set-header name="{{ContosoHeader}}" exists-action="override">
      <value>{{ContosoHeaderValue}}</value>
    </set-header>

Tässä esimerkissä `ContosoHeader` käytetään otsikosta nimenä `set-header` käytännön, ja `ContosoHeaderValue` käytetään kyseisen arvon. Kun käytäntö arvioidaan pyynnön tai vastauksen Ohjelmointirajapinta Management gatewaytä, aikana `{{ContosoHeader}}` ja `{{ContosoHeaderValue}}` korvataan niiden vastaaviin ominaisuusarvoihin.

Ominaisuuksia voi käyttää valmiiksi määrite tai elementtien arvot, kuten edellisessä esimerkissä, mutta ne voivat myös on lisätty tai yhdistää literaaliteksti lausekkeen, kuten seuraavassa esimerkissä osan:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

Ominaisuudet voi sisältää myös käytännön lausekkeita. Seuraavassa esimerkissä `ExpressionProperty` käytetään.

    <set-header name="CustomHeader" exists-action="override">
        <value>{{ExpressionProperty}}</value>
    </set-header>

Kun käytäntö arvioidaan, `{{ExpressionProperty}}` on korvattu arvonsa: `@(DateTime.Now.ToString())`. Arvo on käytännön lauseketta, lauseke arvioidaan ja käytäntö jatkaa sen suorittamisen.

Voit testata tämän ulos developer-portaalissa soittamalla toimintoa, joka on alueen käytäntö, jonka ominaisuuksia. Seuraavassa esimerkissä toiminnon kutsutaan kaksi edellisessä esimerkissä `set-header` käytännöt ominaisuudet. Huomaa, että vastaus sisältää kaksi mukautetut otsikot, jotka on määritetty käytännöt käyttäminen ominaisuudet.

![Developer-portaalissa][api-management-send-results]

Jos tarkastelet [Ohjelmointirajapinta tarkistaminen Jäljitä](api-management-howto-api-inspector.md) kutsun, joka sisältää kaksi edellisen malli-käytäntöjä, ominaisuuksiin, näet kaksi `set-header` tehostaminen lisätty sekä käytännön lauseketta laskennan ominaisuudelle, joka sisältää käytännön lauseketta ominaisuusarvoihin.

![API tarkastaminen seuranta][api-management-api-inspector-trace]

Huomaa, että kun ominaisuusarvoihin voi sisältää käytännön lauseketta, ominaisuusarvoihin ei voi olla muita ominaisuuksia. Jos teksti, joka sisältää viittauksen ominaisuutta käytetään ominaisuuden arvoa, kuten `Property value text {{MyProperty}}`-ominaisuutta, jotka viittaavat ei voi korvata, ja ne ovat ominaisuuden arvon mukaan.

## <a name="to-create-a-property"></a>Luo käyttäjäprofiilille ominaisuus

Voit luoda ominaisuuden, valitse **Ominaisuudet** -välilehdessä **Lisää ominaisuus** .

![Lisää ominaisuus][api-management-properties-add-property-menu]

**Nimen** ja **arvon** ovat pakolliset arvot. Jos tämän ominaisuuden arvon on salainen, valitse **Tämä on salaisuus** -valintaruutu. Kirjoita vähintään yksi valinnaisten tunnisteiden avulla järjestämään että ominaisuudet ja valitse **Tallenna**.

![Lisää ominaisuus][api-management-properties-add-property]

Kun uusi ominaisuus on tallennettu, **haku-ominaisuus** -tekstiruutu lisätään uuden ominaisuuden nimi ja uusi ominaisuus näkyy. Jos haluat näyttää kaikki ominaisuudet, tyhjennä **Etsintä-ominaisuus** -tekstiruutuun ja paina enter.

![Ominaisuudet:][api-management-properties-property-saved]

Lisätietoja REST-Ohjelmointirajapinnan käyttäminen ominaisuus luomisesta on artikkelissa [Luo REST-Ohjelmointirajapinnan käyttäminen ominaisuus](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).

## <a name="to-edit-a-property"></a>Muokkaa ominaisuutta

Muokkaa ominaisuutta, valitse **Muokkaa** ominaisuutta, jos haluat muokata vieressä.

![Ominaisuuksien muokkaaminen][api-management-properties-edit]

Tee haluamasi muutokset ja valitse **Tallenna**. Jos muutat ominaisuuden nimen, käytännöt, jotka viittaavat kyseisestä ominaisuudesta päivittyvät automaattisesti uuden nimeä.

![Ominaisuuksien muokkaaminen][api-management-properties-edit-property]

Lisätietoja REST-Ohjelmointirajapinnan käyttäminen ominaisuus muokkaamisesta on artikkelissa [REST-Ohjelmointirajapinnan käyttäminen ominaisuuksien muokkaaminen](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).

## <a name="to-delete-a-property"></a>Poista ominaisuus

Ominaisuuden poistaminen valitsemalla Poista ominaisuuden vieressä **Poista** .

![Ominaisuuden poistaminen][api-management-properties-delete]

Valitse Vahvista **Kyllä, poista se** .

![Vahvista poistaminen][api-management-delete-confirm]

>[AZURE.IMPORTANT] Jos ominaisuus viitataan käytäntöjä, et voi poistaa sen onnistuneesti, kunnes poistat kaikki käytännöt, jotka käyttävät sitä ominaisuus.

Lisätietoja REST-Ohjelmointirajapinnan käyttäminen ominaisuuden poistamisesta on artikkelissa [REST-Ohjelmointirajapinnan käyttäminen ominaisuuden poistaminen](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).

## <a name="to-search-and-filter-properties"></a>Haku-ja suodattimen ominaisuudet

**Ominaisuudet** -välilehti sisältää hakemisesta ja suodattamisesta ominaisuuksien avulla voit hallita oman ominaisuudet. Haluat suodattaa ominaisuus luettelosta ominaisuuden nimi, kirjoita hakusana **Etsi-ominaisuus** -tekstiruutuun. Jos haluat näyttää kaikki ominaisuudet, tyhjennä **Etsintä-ominaisuus** -tekstiruutuun ja paina enter.

![Haku][api-management-properties-search]

Haluat suodattaa ominaisuus luettelosta tunnisteen arvoja, **suodattamalla tunnisteet** muuttuvat voit kirjoittaa vähintään yksi tunnisteet. Näyttämään kaikki ominaisuudet Tyhjennä- **tunnisteet suodattaminen** tekstiruutu ja paina enter.

![Suodatin][api-management-properties-filter]

## <a name="next-steps"></a>Seuraavat vaiheet

-   Lisätietoja käytännöt käsitteleminen
    -   [Käytännöt API hallinta](api-management-howto-policies.md)
    -   [Käytännön viitata](https://msdn.microsoft.com/library/azure/dn894081.aspx)
    -   [Käytännön lausekkeet](https://msdn.microsoft.com/library/azure/dn910913.aspx)

## <a name="watch-a-video-overview"></a>Katso video yleiskatsaus

> [AZURE.VIDEO use-properties-in-policies]

[api-management-properties]: ./media/api-management-howto-properties/api-management-properties.png
[api-management-properties-add-property]: ./media/api-management-howto-properties/api-management-properties-add-property.png
[api-management-properties-edit-property]: ./media/api-management-howto-properties/api-management-properties-edit-property.png
[api-management-properties-add-property-menu]: ./media/api-management-howto-properties/api-management-properties-add-property-menu.png
[api-management-properties-property-saved]: ./media/api-management-howto-properties/api-management-properties-property-saved.png
[api-management-properties-delete]: ./media/api-management-howto-properties/api-management-properties-delete.png
[api-management-properties-edit]: ./media/api-management-howto-properties/api-management-properties-edit.png
[api-management-delete-confirm]: ./media/api-management-howto-properties/api-management-delete-confirm.png
[api-management-properties-search]: ./media/api-management-howto-properties/api-management-properties-search.png
[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

