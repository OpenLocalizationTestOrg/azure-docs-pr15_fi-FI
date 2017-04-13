<properties 
    pageTitle="Voit luoda ja julkaista tuotteen Azure API hallinta" 
    description="Opettele luominen ja julkaiseminen tuotteiden Azure API hallinta." 
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

# <a name="how-to-create-and-publish-a-product-in-azure-api-management"></a>Voit luoda ja julkaista tuotteen Azure API hallinta

Azure-Ohjelmointirajapinnan hallinta-tuote sisältää vähintään yhden ohjelmointirajapinnan sekä käyttökiintiö ja käyttöehdot. Kun tuote on julkaistu, kehittäjät voit tilata tuotteen ja alkaa käyttää tuotteen ohjelmointirajapinnan. Aihe sisältää tuotteen luominen ja lisääminen Ohjelmointirajapinnan julkaisemisen kehittäjille opas.

## <a name="create-product"> </a>Luo tuote

Toimintojen lisätään ja määritetään API-publisher-portaalissa. Voit käyttää publisher-portaali valitsemalla Azure perinteinen portaalissa API Management-palvelun **hallinta** .

![Publisher-portaalissa][api-management-management-console]

>Jos et ole luonut erillisen service API hallinta-kohdassa [Azure API hallinnan käytön aloittaminen][] -opetusohjelma [Luo erillisen service API hallinta][] .

Voit näyttää **tuotteet** -sivun vasemmassa reunassa olevasta valikosta **tuotteita,** Valitse ja sitten **Lisää tuote**.

![Tuotteet][api-management-products]

![Uuden tuotteen][api-management-add-new-product]

Kirjoita **nimi** -kenttään ja kuvaus **kuvaus** -kenttään tuotteen tuotteen kuvaava nimi.

Tuotteet-Ohjelmointirajapinnan hallinnan voi olla **Avoin** tai **suojattu**. Suojatun tuotteiden oltava tilattuna ennen niiden avulla voidaan ollessa auki tuotteiden voi käyttää ilman tilauksen. Valitse Luo suojatun tuote, joka edellyttää tilauksen **edellyttää tilausta** . Tämä on oletusarvo.

Jos haluat tarkistaa ja hyväksyä tai hylätä tämän tuotteen tilauksen yrittää järjestelmänvalvoja, tarkista **tilauksen hyväksynnän vaatiminen** . Jos ruutu ei ole valittu, tilauksen yritykset on automaattinen hyväksytty. Lisätietoja tilaukset-kohdassa [Näytä tilaajille tuotteen][].

Jos Kehitystyökalut-tilien tilaaminen tuotteen useita kertoja, valitse **Salli useita tilauksia** -valintaruutu. Jos tämä valintaruutu ei ole valittu, developer-tileille voit tilata vain kerran tuotteeseen.

![Rajoittamaton useita tilauksia][api-management-unlimited-multiple-subscriptions]

Useita tilauksia samanaikaisesti määrän rajoittaminen **samanaikaisesti tilaaminen määrän rajoittaminen** -valintaruutu ja kirjoita tilauksen rajoitus. Seuraavassa esimerkissä samanaikaisesti tilaukset on oikeutettu enintään neljä Kehitystyökalut-tiliä.

![Neljä useita tilauksia][api-management-four-multiple-subscriptions]

Kun kaikki uusi tuote-asetukset on määritetty, valitse Luo uusi tuote **Tallenna** .

![Tuotteet][api-management-products-page]

>Oletusarvon mukaan uusien tuotteiden ovat julkaisemattomien, ja ne näkyvät vain **Järjestelmänvalvojat** -ryhmän.

Voit määrittää tuotteen, valitsemalla tuotteen nimi- **tuotteet** -välilehti.

## <a name="add-apis"> </a>API lisätä tuotteeseen

**Tuotteet** -sivu sisältää neljä linkkejä määrittämisessä: **Yhteenveto**, **asetukset**, **näkyvyys**ja **tilaajille**. **Yhteenveto** -välilehti on, jossa voit lisätä ohjelmointirajapinnan ja julkaiseminen tai julkaisun tuotteen.

![Yhteenveto][api-management-new-product-summary]

Ennen kuin julkaiset tuotteesi haluat lisätä vähintään yhden API. Valitse **Lisää API tuotteeseen**.

![Lisää API][api-management-add-apis-to-product]

Valitse haluamasi ohjelmointirajapinnan ja valitse **Tallenna**.

## <a name="add-description"> </a>Tuotteen kuvaava tietojen lisääminen

**Asetukset** -välilehdessä voit on tarkkoja tietoja tuotteen, kuten tarkoitustaan, se on pääsy ohjelmointirajapinnan ja muita hyödyllisiä tietoja. Sisältö on suunnattu kehittäjät, joka soittavan Ohjelmointirajapinnan ja voidaan kirjoittaa vain teksti-tai HTML-koodi.

![Tuotteen asetukset][api-management-product-settings]

Valitse Luo suojatun tuote, joka edellyttää, jota käytetään tilauksen **edellyttää tilausta** tai Avaa tuote, joka voidaan kutsua ilman tilauksen luominen-valintaruudun valinta.

Valitse **tilauksen hyväksynnän vaatiminen** , jos haluat hyväksyä kaikki tuotteen tilauksen pyynnöt manuaalisesti. Oletusarvon mukaan kaikkien tuotteen tilausten myönnetään automaattisesti.

Jos Kehitystyökalut-tilien tilaaminen tuotteen useita kertoja, **Salli useita tilauksia** -valintaruutu ja määritä myös rajoitukset. Jos tämä valintaruutu ei ole valittu, developer-tileille voit tilata vain kerran tuotteeseen.

Voit myös Täytä **käyttöehdot** -kenttään kuvaava product-tilaajille, jotka on hyväksyttävä voi käyttää tuotteen käyttöehdot.

## <a name="publish-product"> </a>Tuotteen julkaiseminen

Ennen kuin tuotteen API voidaan kutsua, tuote on oltava julkaistu. Tuotteen **Yhteenveto** -välilehden **Julkaise**ja valitse sitten Vahvista **Kyllä, julkaisemaan** . Aiemmin julkaistut tuotteen yksityisiksi Poista **julkaisu**.

![Julkaise tuote][api-management-publish-product]

## <a name="make-visible"> </a>Tee tuotteen näkyvä kehittäjät

**Näkyvyys** -välilehdessä voit valita, mitkä roolit ovat voivat nähdä tuotteen developer-portaalissa ja tuotteen tilaa.

![Tuotteen näkyvyys][api-management-product-visiblity]

Ottaa käyttöön tai poistaa tuotteen näkyvyyden ryhmän kehittäjille, tarkista tai poista ryhmän vieressä oleva valintaruutu ja valitse sitten **Tallenna**.

>Lisätietoja on artikkelissa [miten voit luoda ja käyttää ryhmiä Azure API hallinnan developer tilien hallinta][].

## <a name="view-subscribers"> </a>Tuotteen tilaajille tarkasteleminen

**Tilaajille** -välilehdessä näkyvät sovelluskehittäjät, jotka on määritetty vastaanottamaan tuotteen. Jokainen kehittäjä asetukset ja tiedot voidaan tarkastella valitsemalla kehittäjän nimi. Tässä esimerkissä kehittäjät ei ole vielä tilannut tuotteen.

![Kehittäjät][api-management-developer-list]

## <a name="next-steps"> </a>Seuraavat vaiheet

Kun haluamasi ohjelmointirajapinnan on lisätty ja tuotteen julkaistu, kehittäjät voit tilata tuotteen ja aloittaa Soita API. Opetusohjelmaan, joka esittelee nämä kohteet sekä Lisäasetukset tuotteen määritys tarkistaa, [kuinka luominen ja Lisäasetukset tuotteen Azure API hallinnan asetusten määrittäminen][].

Lisätietoja tuotteiden käyttämisestä on seuraavassa videossa.

> [AZURE.VIDEO using-products]

[Create a product]: #create-product
[Add APIs to a product]: #add-apis
[Add descriptive information to a product]: #add-description
[Publish a product]: #publish-product
[Make a product visible to developers]: #make-visible
[Näytä tilaajille tuotteeseen]: #view-subscribers
[Next steps]: #next-steps

[api-management-management-console]: ./media/api-management-howto-add-products/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-add-products/api-management-add-product.png
[api-management-add-new-product]: ./media/api-management-howto-add-products/api-management-add-new-product.png
[api-management-unlimited-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-unlimited-multiple-subscriptions.png
[api-management-four-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-four-multiple-subscriptions.png
[api-management-products-page]: ./media/api-management-howto-add-products/api-management-products-page.png
[api-management-new-product-summary]: ./media/api-management-howto-add-products/api-management-new-product-summary.png
[api-management-add-apis-to-product]: ./media/api-management-howto-add-products/api-management-add-apis-to-product.png
[api-management-product-settings]: ./media/api-management-howto-add-products/api-management-product-settings.png
[api-management-publish-product]: ./media/api-management-howto-add-products/api-management-publish-product.png
[api-management-product-visiblity]: ./media/api-management-howto-add-products/api-management-product-visibility.png
[api-management-developer-list]: ./media/api-management-howto-add-products/api-management-developer-list.png



[api-management-products]: ./media/api-management-howto-add-products/api-management-products.png
[api-management-]: ./media/api-management-howto-add-products/
[api-management-]: ./media/api-management-howto-add-products/


[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[Azure API hallinnan käytön aloittaminen]: api-management-get-started.md
[Luo erillisen service API hallinta]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps
[Voit luoda ja hallita developer Azure API hallinnan ryhmien avulla]: api-management-howto-create-groups.md
[Miten luominen ja Azure API hallinnan lisäasetukset tuotteen asetusten määrittäminen]: api-management-howto-product-with-rules.md 