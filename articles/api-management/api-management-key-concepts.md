<properties 
    pageTitle="Avaimen käsitteitä API hallinta" 
    description="Lisätietoja API, tuotteiden, roolit, ryhmät ja muut API hallinta avaimen käsitteitä." 
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
    ms.topic="hero-article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

#<a name="what-is-api-management"></a>Mikä on API hallinta?

API hallinta auttaa organisaatioita julkaistava ohjelmointirajapinnan ulkopuolisille, kumppaneiden ja sisäinen kehittäjät lukitus mahdollisuuksia tietoja ja -palveluita. Yritysten kaikkialla ovat Haluatko laajentaa kuin digitaalinen ympäristö, uusi kanavien luomisesta, uusien asiakkaiden etsiminen ja ajo tarkempaa välitys ja aiemmin luotuja. API hallinta sisältää core osaamisalueiden varmistaa onnistuneen API-ohjelman kautta developer välitys, yrityksen tiedot, analytics, tietoturva ja suojaus.

Seuraavassa videossa Azure API hallinnan yleiskatsaus ja opi API hallinnan avulla voit lisätä useita ominaisuuksia API, esimerkiksi käyttöoikeuksien valvonta korko rajoittaminen, seuranta, tapahtumaloki ja vastaus välimuistiin mahdollisimman vähän ja muutokset.

> [AZURE.VIDEO azure-api-management-overview]

API hallinta käyttäminen Järjestelmänvalvojat luovat API. Kunkin API koostuu vähintään yksi toiminnot ja kunkin API voidaan lisätä yksi tai useita tuotteita. Voit käyttää Ohjelmointirajapinnan, kehittäjät tilaaminen tuote, joka sisältää kyseisen API ja ne sitten Soita Ohjelmointirajapinnan-toiminnon käyttö käytäntöjä, jotka saattavat olla voimassa veloittaa.

Tämä artikkeli sisältää yleiskatsauksen API hallinta avaimen käsitteitä.

>[AZURE.NOTE] Lisätietoja on artikkelissa [pilvipohjainen API hallinta: valtio ohjelmointirajapinnan Power](http://j.mp/ms-apim-whitepaper) julkaisu PDF. Tämä johdanto julkaisu-Ohjelmointirajapinnan hallitsemaan CITO Oheistiedot kuuluvat: 
>
> - Yhteiset API vaatimukset ja haasteita
>     - Irtautus ohjelmointirajapinnan ja esittäminen facades
>     - Kehittäjät käytön ja suorittamalla nopeasti
>     - Käytön suojaamisesta
>     - Analyysin ja arvot
> - Kasvun hallita ja ymmärtää API hallinta-ympäristön kanssa
> - Cloud ja paikallinen ratkaisujen käyttäminen
> - Azure API hallinta

## <a name="apis"> </a>API ja toiminnot

API ovat foundation API hallinta esiintymän. Kunkin API edustaa joukko toimintoja, jotka ovat käytettävissä kehittäjille. Kunkin API sisältää taustatietokantaan palvelu, joka sisältää Ohjelmointirajapinnan viittaus ja sen toimintojen kartan taustatietokantaan-palvelu käytössä toimintoihin. API hallinnan toiminnot ovat erittäin määritettävissä hallita tietueina URL-osoite yhdistämistä, kyselyn ja polku parametrit pyynnön ja vastauksen sisällön tai toiminnon vastauksen välimuistiin. Raja kerroin kiintiön ja IP-rajoituskäytännöt myös voidaan toteuttaa API tai yksittäisistä tasolla.

Lisätietoja on artikkelissa [luomisesta ohjelmointirajapinnan][] sekä [lisätä toimintoja API][].


## <a name="products"></a> Tuotteet

Tuotteet ovat, miten ohjelmointirajapinnan esiin kehittäjille. Tuotteiden API hallinnassa on vähintään yksi ohjelmointirajapinnan ja on määritetty nimi, kuvaus ja käyttöehdot. Tuotteiden voi olla **Avoin** tai **suojattu**. Suojatun tuotteiden oltava tilattuna ennen niiden avulla voidaan ollessa auki tuotteiden voi käyttää ilman tilauksen. Kun tuote on valmis käytettäväksi sovelluskehittäjät se voidaan julkaista. Kun se on julkaistu, se voi tarkastella (ja suojatun tuotteiden tilannut) sovelluskehittäjät. Tilauksen hyväksynnän on määritetty tuotteen tasolla ja voit joko järjestelmänvalvojan on hyväksyttävä tai olla automaattinen hyväksytty.

Ryhmien käytetään tuotteet kehittäjille näkyvyyden hallinta. Tuotteiden myöntäminen näkyvyyden ryhmiä ja kehittäjät voivat tarkastella ja tilata tuotteita, jotka ovat näkyvissä ryhmiin, johon ne kuuluvat. 

Katso lisätietoja, [miten voit luoda ja julkaista tuote][] - ja seuraavassa videossa.

> [AZURE.VIDEO using-products]

## <a name="groups"></a> Ryhmät

Ryhmien käytetään tuotteet kehittäjille näkyvyyden hallinta. API hallinta on seuraavat pysyvä järjestelmän ryhmät.

-   **Järjestelmänvalvojat** - Azure tilauksen järjestelmänvalvojat voivat tämän ryhmän jäsenet. Järjestelmänvalvojat hallita API Management-palvelun esiintymät, API, toiminnot ja tuotteet, joita käytetään sovelluskehittäjät luominen.
-   **Kehittäjät** - ryhmään kuuluvat käyttäjien todennetut developer-portaalissa. Kehittäjät ovat asiakkaat, joilla käyttämällä omaa API-sovelluksia. Kehittäjät ovat developer-portaalissa käyttöoikeudet, ja kutsu Ohjelmointirajapinnan toimintoja sovellusten rakentamiseen.
-   Ryhmään kuuluvat **vierailijat** - Todentamattomille developer portaalin käyttäjille, kuten mahdollisille asiakkaille ohjesisältöä API hallinta-esiintymän developer-portaalissa. Hän voi myöntää tiettyjä vain luku-tilassa, kuten mahdollisuus tarkastella API, mutta ne kutsu.

Lisäksi järjestelmän ryhmiin järjestelmänvalvojat voivat luoda mukautettuja ryhmiä tai [hyödyntää liittyvät Azure Active Directory-alihallinnat ulkoisen ryhmät](api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group). Mukautettu ja ulkoiset ryhmät voidaan järjestelmän ryhmien kehittäjät näkyvyyttä ja Accessin antaminen API tuotteiden rinnalla. Voit esimerkiksi yhden mukautetun ryhmän luominen liittyvän tietyn kumppaniorganisaation kehittäjille ja Salli ne käyttämistä API tuote, joka sisältää haluamasi API. Käyttäjä voi olla useita ryhmän jäsen.

Lisätietoja on artikkelissa [miten voit luoda ja käyttää ryhmiä][].

## <a name="developers"></a> Kehittäjät

Kehittäjät edustavat käyttäjätilit API Management-palvelun esiintymän. Kehittäjät voidaan luoda tai on kutsuttu liittymään järjestelmänvalvojien tai he voivat allekirjoittaa [Developer-portaalissa][]. Jokainen kehittäjä on yksi tai useita ryhmiä jäsen, ja se voi olla tilaaminen tuotteet, joita näkyvyyden myöntäminen ryhmille.

Kun kehittäjät tilaaminen tuotteen ne myönnetään tuotteen ensisijainen ja toissijainen-näppäintä. Tätä näppäintä käytetään, kun puheluiden soittamiseen tuotteen ohjelmointirajapinnan kyselyjä.

Lisätietoja on artikkelissa [luomisesta ja kutsu kehittäjät][] ja [niiden liittää ryhmissä, joissa on kehittäjät][].

## <a name="policies"></a> Käytännöt

Käytännöt ovat tehokas ominaisuuksien API hallinnan, jotka sallivat Publisherin määrittämisen jälkeen Ohjelmointirajapinnan toiminnan muuttaminen. Käytännöt ovat lausekkeita, jotka suoritetaan järjestyksessä pyynnön kokoelma tai Ohjelmointirajapinnan vastaus. Suositut lauseet ovat tiedostomuodon muuntaminen XML: stä JSON ja puhelun korko rajoittaminen kehittäjä Saapuvien puhelujen määrän rajoittaminen ja monia muita käytännöt ovat käytettävissä.

Käytännön lausekkeiden voidaan määritearvojen tai tekstiarvojen kaikista API hallintakäytännöt, ellei käytäntö määrittää muuten. Joitakin käytäntöjä, kuten [Hallintavuo](https://msdn.microsoft.com/library/azure/dn894085.aspx#choose) ja [Määritä muuttujan](https://msdn.microsoft.com/library/azure/dn894085.aspx#set-variable) käytännöt perustuvat käytännön lausekkeita. Lisätietoja on artikkelissa [Advanced käytäntöjä](https://msdn.microsoft.com/library/azure/dn894085.aspx#AdvancedPolicies), [käytäntö lausekkeista](https://msdn.microsoft.com/library/azure/dn910913.aspx), ja katso seuraava video.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

Katso luettelo kaikista API hallintakäytännöt, [käytännön viitata][]. Katso lisätietoja käyttämisestä ja määrittämisestä käytäntöjä, [API hallintakäytäntöjä][]. Opetusohjelmaan luomisesta tuotteen arvolla raja-ja kiintiötiedot tarkistaa, [kuinka luominen ja Lisäasetukset tuotteen asetusten määrittäminen][]. Katso esittely, seuraavassa videossa.

> [AZURE.VIDEO rate-limits-and-quotas]

## <a name="developer-portal"></a> Developer-portaalissa

Developer-portaalissa on missä kehittäjät voivat API, näkymä ja puhelun toimintojen tietoja ja tuotteiden tilaaminen. Mahdollisille asiakkaille käsiksi developer-portaalissa ohjelmointirajapinnan ja toimintojen tarkasteleminen ja tilaa. Raporttinäkymät-ikkunan Azure perinteinen portaalissa API Management-palvelun esiintymän sijaitsee Kehitystyökalut-portaalin URL-osoite.

Voit mukauttaa developer-portaalissa ulkoasun lisäämällä mukautettua sisältöä, tyylin mukauttaminen ja lisätä oman yrityksesi tunnuksen.

## <a name="api-management-and-the-api-economy"></a>API hallinta ja API taloudessa

Lisätietoja API hallinta, Katso seuraavat esityksen Microsoft Ignite 2015 neuvottelusta.

> [AZURE.VIDEO microsoft-ignite-2015-azure-api-management-and-the-api-economy]

[APIs and operations]: #apis
[Products]: #products
[Groups]: #groups
[Developers]: #developers
[Policies]: #policies
[Developer-portaalissa]: #developer-portal

[API luominen]: api-management-howto-create-apis.md
[Toimintojen lisääminen Ohjelmointirajapinta]: api-management-howto-add-operations.md
[Voit luoda ja julkaista tuote]: api-management-howto-add-products.md
[Voit luoda ja käyttää ryhmiä]: api-management-howto-create-groups.md
[Miten ryhmät liitetään kehittäjät]: api-management-howto-create-groups.md#associate-group-developer
[Miten luominen ja Lisäasetukset tuotteen asetusten määrittäminen]: api-management-howto-product-with-rules.md
[Luomisesta ja kutsu kehittäjät]: api-management-howto-create-or-invite-developers.md
[Käytännön viitata]: api-management-policy-reference.md
[API hallintakäytäntöjen tuki]: api-management-howto-policies.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance



 
