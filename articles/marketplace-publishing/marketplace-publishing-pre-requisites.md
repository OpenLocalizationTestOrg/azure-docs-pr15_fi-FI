<properties
   pageTitle="Tarjouksen luominen Azure Marketplacen tehtävistä selainpohjaisista edellytyksistä | Microsoft Azure"
   description="Tietoja luominen ja käyttöönotto tarjouksen Azure Marketplacesta muiden ostaa koskevat vaatimukset."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
  ms.service="marketplace"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="Azure"
  ms.workload="na"
  ms.date="08/18/2016"
  ms.author="hascipio"/>

# <a name="general-prerequisites-for-creating-an-offer-for-the-azure-marketplace"></a>Yleiset edellytyksistä tarjouksen luominen Azure Marketplace
Hyvä tietää Yleiset, business-prosessin keskitettyä edellytykset, joita tarvitaan teosta tarjouksen luontiprosessi.

## <a name="ensure-that-you-are-registered-as-a-seller-with-microsoft"></a>Varmista, että on rekisteröity Microsoft myyjä
Siirry tarkat ohjeet myyjä-tilin rekisteröiminen Microsoft- [tilin luominen ja rekisteröintiä](marketplace-publishing-accounts-creation-registration.md).

- **Jos yrityksesi on jo rekisteröity myyjän keskihajonta keskelle ja haluat luoda uusi tarjous,** valitse Kirjautuminen julkaisu portaalin, jolla on sama sähköpostitunnus, mitkä keskihajonta Centerin rekisteröinti on valmis. Tässä vaiheessa vaaditaan, jotta keskihajonta Center ja julkaiseminen-portaalissa on linkitetty toisiinsa.
- **Jos yrityksesi on jo rekisteröity myyjän keskihajonta keskelle ja haluat muokata aiemmin tarjous,** ja valitse joko julkaisu kirjautuessasi portaalin järjestelmänvalvojatilillä tai käyttämällä tiliä, johon on lisätty julkaisu Mää-järjestelmänvalvojaksi portal. Mää järjestelmänvalvojan tilin lisäämisen vaiheet on alla.

## <a name="steps-to-add-a-co-admin-in-the-publishing-portal"></a>Vaiheet julkaisun portaalissa Mää-järjestelmänvalvojan lisääminen
Järjestelmänvalvojat, julkaisu portaalissa voit lisätä muiden jäsenten yrityksen, jotka työskentelevät sovellus, julkaisu Mää-järjestelmänvalvojaksi portal. **Oletetaan, että olet järjestelmänvalvoja,** alla ovat tavalla kuin lisäät Mää-järjestelmänvalvoja.

>[AZURE.NOTE] Uusille käyttäjille, ennen kuin voit lisätä Mää-järjestelmänvalvojan julkaisu-portaaliin, varmista, että olet luonut vähintään yksi sovellus julkaisu portal. Tämä on pakollinen **JULKAISIJAT** -välilehdessä näkyvät vain, kun olet luonut vähintään yksi sovellus julkaisu portal.

1. Varmista, että Mää järjestelmänvalvojan sähköpostitunnus Microsoft account(MSA). Jos et rekisteröi se MSA, voit käyttää tätä [linkkiä](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1).
2. Varmista, että on oltava vähintään yksi sovellus järjestelmänvalvoja-tilissä ennen kuin yrität lisätä Mää-järjestelmänvalvoja.
3. Kun edellä kuvatut toimet on valmis, kirjaudu julkaisu portaalin Mää järjestelmänvalvojan sähköpostitunnus ja sitten ulos.
4. Kirjaudu nyt julkaisu portaalin järjestelmänvalvoja sähköpostin tunnuksella.
5. Siirry julkaisijat-tilisi -> Valitse > järjestelmänvalvojat -> Lisää Mää-admin (näyttökuva alla)

    ![piirustuksen](media/marketplace-publishing-pre-requisites/imgAddAdmin_05.png)

6. Varmista annettu julkaisuprosessin (kuten keskihajonta Center-portaalin julkaiseminen) eri vaiheissa sähköpostitunnuksia seurataan Microsoftin osalta.
7. Keskihajonta Center rekisteröinnin välttää käyttämällä tiliä, joka on liitetty yhden henkilön kanssa. Tämä on ehdotettu riippuvuuden poistaminen yksi henkilö.
8. Jos keskihajonta Center rekisteröinti ongelmia yleisölle tarkoitetun, valitse Ota korottaa lippu tämän [linkin](https://developer.microsoft.com/en-us/windows/support)avulla.

## <a name="steps-to-delete-a-co-admin-in-the-publishing-portal"></a>Poistaminen Mää-järjestelmänvalvojan julkaisu-portaalissa
**Oletetaan, että olet järjestelmänvalvoja,** alla on ohjeet poistaminen Mää-järjestelmänvalvoja.

1. Kirjautuminen julkaisu portaalin järjestelmänvalvoja sähköpostin tunnuksella.
2. Siirry **julkaisijat** -> Valitse tili -> **Järjestelmänvalvojat** -> **Apuyhteyshenkilöiden**.
3. Valitse **X** -painiketta seuraava Mää-järjestelmänvalvojan haluat poistaa tot (näyttökuva alla).

    ![piirustuksen](media/marketplace-publishing-pre-requisites/imgDeleteAdmin_03.png)

> [AZURE.IMPORTANT] Ei tarvitse suorittaa vero ja bank Yritystiedot, jos aiot julkaista vain ilmaisia tarjouksia (tai tuoda Omat käyttöoikeus).

> Yrityksen rekisteröinti on valmis aloittamaan. Kuitenkin samalla, kun yrityksen toimii Microsoft Developer-tilin arvonlisävero ja pankkitilin tietoja, kehittäjät aloittaa työskentelyn luominen virtuaalikoneen kuvan [Julkaisemisportaali](https://publish.windowsazure.com), käytön sen varmennettu ja testaus Azure väliaikaisen-ympäristössä. Sinun on valmis myyjän tilin hyväksyminen malleissa viimeisessä vaiheessa tarjous julkaiseminen Azure Marketplacesta.

## <a name="acquire-an-azure-pay-as-you-go-subscription"></a>Azure "tukiyhteyksiä" tilauksen muokkaaminen
Tämä on tilaus, voit luoda AM kuvat ja jakaa päälle kuvat [Azure Marketplacesta](https://azure.microsoft.com/marketplace/). Jos sinulla ei ole olemassa olevaan tilaukseen, valitse Kirjaudu sisään osoitteessa https://account.windowsazure.com/signup?offer=ms-azr-0003p.

## <a name="sell-from-countries"></a>"Myynti-lähettäjä" maat
> [AZURE.WARNING]
Jotta myydä Azure Marketplace-palvelujen, sinun on tehtävä että rekisteröity kohde on hyväksytty "Myynti-lähettäjä"-maista olevista. Tämä rajoitus koskee maksu ja verojen syistä. Olemme aktiivisesti näyttöä laajenna luetteloa maiden lähitulevaisuudessa, joten on tulossa. Katso kattavaan luetteloon, osa 1 b [Azure Marketplacesta osallistuminen käytännöt](http://go.microsoft.com/fwlink/?LinkID=526833).

## <a name="next-steps"></a>Seuraavat vaiheet
Kun tehtävistä selainpohjaisista vaatimukset täyttyvät, seuraavaksi edellytyksiä tarjous tietyn tekniset. Napsauta linkkiä, jotka haluat luoda Azure Marketplacen vastaaviin tarjouksen osalta artikkeliin.

- [AM tekniset vaatimukset](marketplace-publishing-vm-image-creation-prerequisites.md)
- [Ratkaisu mallin tekniset vaatimukset](marketplace-publishing-solution-template-creation-prerequisites.md)

## <a name="see-also"></a>Katso myös
- [Aloittaminen: julkaiseminen tarjouksen Azure Marketplacesta](marketplace-publishing-getting-started.md)
