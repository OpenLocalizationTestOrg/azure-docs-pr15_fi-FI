<properties
   pageTitle="Pikaopas Azure AD-kaavion API | Microsoft Aure"
   description="Azure Active Directory Graph-Ohjelmointirajapinnan tarjoaa ohjelmallisesti Azure AD OData REST API päätepisteet kautta. Sovellukset voivat käyttää Graph-Ohjelmointirajapinnan suorittamiseen luominen, lukeminen, päivittäminen ja poistaminen (CRUD) toimenpiteet directory tietoja ja objekteja."
   services="active-directory"
   documentationCenter="n/a"
   authors="PatAltimore"
   manager="mbaldwin"
   editor=""
   tags=""/>


   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="09/16/2016"
      ms.author="patricka"/>

# <a name="quickstart-for-the-azure-ad-graph-api"></a>Azure AD-kaavion API pikaopas

Azure Active Directory (AD) Graph Ohjelmointirajapinnan tarjoaa ohjelmallisesti Azure AD OData REST API päätepisteet kautta. Sovellukset voivat käyttää Graph-Ohjelmointirajapinnan suorittamiseen luominen, lukeminen, päivittäminen ja poistaminen (CRUD) toimenpiteet directory tietoja ja objekteja. Voit esimerkiksi käyttää Graph-Ohjelmointirajapinnan Luo uusi käyttäjä, tarkastella tai päivittää käyttäjän ominaisuuksia, käyttäjän salasanan vaihtaminen, tarkista Ryhmäjäsenyyden Roolipohjainen käytön, poistetaan käytöstä tai Poista käyttäjä. Lisätietoja kaavion API ominaisuuksiin ja sovelluksen tilanteita, joissa on artikkelissa [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) ja [Azure AD Graph API edellytykset](https://msdn.microsoft.com/library/hh974476.aspx). 

> [AZURE.IMPORTANT] Azure AD-kaavion API toiminnot ovat käytettävissä myös [Microsoft Graph](https://graph.microsoft.io/)-yhdistetyn API, joka sisältää API muiden Microsoft-palveluiden, kuten Outlook, OneDrive, OneNote, suunnittelusta ja Office Graph-käytettävissä yhden päätepisteen avulla ja yksittäisen käyttöoikeustietue kautta.

## <a name="how-to-construct-a-graph-api-url"></a>Voit käyttää kaavion API URL-osoite

Kaavion API jotta voit käyttää hakemiston tietoja ja objekteja (toisin sanoen resursseja tai yksiköt) vastaan, jonka haluat suorittaa CRUD-toimintoja, voit käyttää URL-osoitteet Open Data (OData)-protokollaa perusteella. URL-osoitteet-kaavion API koostua neljä pääosaa: palvelun ylimmällä tasolla, vuokraajan tunnus tai resurssipolku merkkijonon Kyselyasetukset: `https://graph.windows.net/{tenant-identifier}/{resource-path}?[query-parameters]`. Ota seuraava URL-osoite Esimerkki: `https://graph.windows.net/contoso.com/groups?api-version=1.6`.

- **Palvelun pääkansio**: Azure AD-kaavion API-palvelun pääkansio on aina https://graph.windows.net.
- **Vuokraajan tunnus**: Tämä voi olla varmennettu (rekisteröity) toimialuenimi, contoso.com yllä olevassa esimerkissä. Se voi olla myös vuokraajan Objektitunnus tai "myorganiztion" tai "minä" alias. Lisätietoja on artikkelissa [osoitteiden yksiköt ja toimintojen Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview)).
- **Resurssipolku**: tätä URL-Osoitteen osa määrittää resurssin projektitietoja käyttämällä (käyttäjät, ryhmät, tietyn käyttäjän tai olevaan ryhmään, jne.) Yllä olevassa esimerkissä on osoitteeseen, joka resurssille on määritetty ylimmän tason "ryhmät". Voit myös osoite tietyn kohteen, esimerkiksi "käyttäjien / {objektitunnus}" tai "käyttäjien/userPrincipalName".
- **Kyselyparametrit**:? erottaa kyselyn parametrit-osasta resurssin polku-osasta. "Api-version" kyselyn parametri tarvitaan Graph-Ohjelmointirajapinnan kaikki pyynnöt. Graph-Ohjelmointirajapinnan tukee myös OData Kyselyasetukset: **$filter**, **$orderby**, **$Laajenna**, **$top**ja **$format**. Seuraavat Kyselyasetukset eivät ole tuettuja: **$count**, **$inlinecount**ja **$skip**. Lisätietoja on artikkelissa [Tuetut kyselyt, suodattimet ja Azure AD Graph API sivutus asetukset](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options).

## <a name="graph-api-versions"></a>Kaavion API-versiot

"Api-version" kyselyn parametri määritetään Graph API-pyynnössä versio. 1,5 ja uudempi versio Käytä numeerisia versio arvo. API-version = 1.6. Aiemmissa versioissa voit käyttää päivämäärä-merkkijono, joka noudattaa YYYY-MM-DD-muotoon esimerkiksi api-versio = 2013 11-08. Esikatselu-ominaisuudet Käytä merkkijonon "beta"; esimerkiksi api-versio beeta =. Saat lisätietoja kaavion API-versioiden väliset erot [Azure AD Graph API versiotietojen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning).

## <a name="graph-api-metadata"></a>Graph API-metatiedot

Palauttaa Graph API metatieto-tiedosto, lisäämällä vuokraajan-tunniste URL-osoite, esimerkiksi "$metadata"-osion, seuraava URL-osoite palauttaa metatietojen Graph Explorerin käyttämiä esittely yrityksen: `https://graph.windows.net/GraphDir1.OnMicrosoft.com/$metadata?api-version=1.6`. Voit kirjoittaa tämän URL-Osoitteen Nähdäksesi metatiedot selaimen osoiteriville. Palauttaa CSDL metatietojen asiakirja kuvataan kohteiden ja monimutkaisia tyypit, niiden ominaisuudet ja toiminnot ja näyttämiä Graph API pyydetty version toimintoja. Pois jättäminen api-versio-parametri palauttaa metatiedot uusimman version.

## <a name="common-queries"></a>Yleiset kyselyt

[Azure AD Graph API Yleiset kyselyt](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options#CommonQueries) on lueteltu yleisiä kyselyitä, jotka voidaan käyttää Azure AD-kaavio, mukaan lukien, jonka avulla voidaan käyttää ylimmän tason resursseja hakemistossa kyselyjen ja suorittaa hakemistossa.

Esimerkiksi `https://graph.windows.net/contoso.com/tenantDetails?api-version=1.6` palauttaa yrityksen tietoja directory contoso.com.

Tai `https://graph.windows.net/contoso.com/users?api-version=1.6` on lueteltu kaikki käyttäjän objektit directory-toimialueen contoso.com.

## <a name="using-the-graph-explorer"></a>Kaavion Resurssienhallinnassa

Azure AD-kaavio-API Graph Resurssienhallinnan avulla voit kysely directory-tiedot, kun luot sovelluksen.

> [AZURE.IMPORTANT] Kaavion Explorer ei tue kirjoittamisen tai poistamisen hakemistosta. Voit tehdä vain luku Azure AD-kansio Resurssienhallinnassa Graph-toimintoja.

Seuraavassa on tulos näkyy jos olit Siirry Graph Resurssienhallinta, valitse Käytä esittely yrityksen ja syötä `https://graph.windows.net/GraphDir1.OnMicrosoft.com/users?api-version=1.6` näytettävä kaikille käyttäjille esittely hakemiston:

![Azure AD graph api explorer](./media/active-directory-graph-api-quickstart/graph_explorer.png)

**Lataa Graph Explorer**: lataamisesta työkalua, siirry [https://graphexplorer.cloudapp.net/](https://graphexplorer.cloudapp.net/). Valitse **Käytä esittely yrityksen** suorittamaan Graph Explorer otoksen vuokraajasta vastaan tiedot. Sinun ei tarvitse käytettävät esittely yrityksen tunnistetiedot. Voit vaihtoehtoisesti valitsemalla **Kirjaudu sisään** ja kirjaudu sisään Azure AD-tilin tunnistetiedot suorittaa Graph Explorer vastaan alihallintaan. Jos suoritat Graph Explorer oman vuokraajan, sinä tai järjestelmänvalvoja on aikana-kirjautuminen sallia. Jos sinulla on Office 365-tilaus, sinun on automaattisesti Azure AD-vuokraajan. Voit käyttää Office 365: een kirjautuminen tunnistetiedot ovat itse asiassa Azure AD-tilejä, ja voit käyttää näitä tunnistetietoja Graph Resurssienhallinnassa.

**Kyselyn suorittaminen**: Jos haluat suorittaa kyselyn, Kirjoita kyselyn pyynnön-tekstiruutuun ja valitse **HAE** tai valitsemalla **Anna** avain. Tulokset näkyvät vastaus-ruutuun. Esimerkiksi `https://graph.windows.net/graphdir1.onmicrosoft.com /groups?api-version=1.6` sisältyviä esittely-kansion kaikki ryhmään objekteja.

Huomaa seuraavat ominaisuudet ja rajoitukset Graph Explorer:
- Automaattisen täydennyksen ominaisuuksien resurssin määrittää. Voit tarkastella, valitsemalla **Käytä esittely yrityksen** ja valitse sitten pyynnön tekstiä (jossa yrityksen URL-osoite näkyy). Voit valita resurssin määrittäminen avattavasta luettelosta.

- Tukee "minä" ja "myorganization" osoitteen tunnukset. Voit esimerkiksi käyttää `https://graph.windows.net/me?api-version=1.6` palauttaa käyttäjäobjektin kirjautunut sisään käyttäjän tai `https://graph.windows.net/myorganization/users?api-version=1.6` palauttaa nykyisen kansion kaikki käyttäjät. Huomaa, että käyttämällä "minä" alias palauttaa virheen esittely yrityksen tavallista sivua ei ole kirjautunut sisään-käyttäjän pyynnön.

- Vastauksen otsikot osa. Tämä voidaan käynnissä kyselyjen yhteydessä ilmenevien ongelmien vianmääritystä.

- Laajenna ja Tiivistä ominaisuuksia vastauksen JSON kehyksen katseluohjelmassa.

- Ei tukea näyttäminen pikkukuva.

## <a name="using-fiddler-to-write-to-the-directory"></a>Kirjoita hakemiston Fiddler avulla

Tämä pika-aloitusopas yhdistämiskyselyssä voit käyttää Fiddler Web virheenkorjaus jotta menestyneet kirjoittaa' toimintojen vastaan voit harjoitella Azure Active directory. Lisätietoja ja asenna Fiddler on artikkelissa [http://www.telerik.com/fiddler](http://www.telerik.com/fiddler).

Seuraavassa esimerkissä käytetään Fiddler Web virheenkorjaus luoda uuden käyttöoikeusryhmän 'MyTestGroup' Azure AD-kansioon.

**Hae käyttöoikeustietue**: Azure AD Graphin käyttöoikeudet asiakkaiden tarvitaan onnistuneesti todentaa Azure AD ensin. Lisätietoja on artikkelissa [Azure AD todennus tilanteita, joissa](active-directory-authentication-scenarios.md).

**Luo ja suorita kysely**: seuraavien vaiheiden mukaisesti.

1. Avaa Fiddler Web virheenkorjaus ja **tunnus** -välilehteen.
2. Jälkeen, kun haluat luoda uuden käyttöoikeusryhmän, valitse **viestin** salaus puretaan avattavasta valikosta HTTP-menetelmää. Lisää toimintoja ja käyttöoikeudet-ryhmän objektin, katso lisätietoja [Azure AD Graph REST API-viittaus](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) [ryhmän](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#GroupEntity) .
3. Kirjoita **viestin**vieressä-kenttään seuraavassa pyyntö URL-osoitteena: `https://graph.windows.net/mytenantdomain/groups?api-version=1.6`.

    > [AZURE.NOTE] Korvaa mytenantdomain Azure AD-hakemistossa toimialueen nimi.

4. Kirjoita alapuolella viestin avattava-kenttään seuraavasti:

    ```
Host: graph.windows.net
Authorization: your access token
Content-Type: application/json
```

    > [AZURE.NOTE] Vaihda käyttäjän &lt;access-tunnuksen&gt; käyttöoikeustietue Azure AD-kansion kanssa.

5. Kirjoita **pyytää leipäteksti** -kenttään seuraavasti:

    ```
        {
            "displayName":"MyTestGroup",
            "mailNickname":"MyTestGroup",
            "mailEnabled":"false",
            "securityEnabled": true
        }
```

    Lisätietoja ryhmien luomisesta on artikkelissa [Luo ryhmä](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#CreateGroup).

Lisätietoja Azure AD-yksiköt ja tyypit, jotka ovat näyttämiä Graph ja tietoja toiminnoista, joita voi käyttää niitä Graphin kanssa on artikkelissa [Azure AD Graph REST API-viittaus](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [Azure AD-kaavio-Ohjelmointirajapinta](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)
- Lisätietoja [Azure AD Graph API käyttöoikeuksien käyttöalueet](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)
