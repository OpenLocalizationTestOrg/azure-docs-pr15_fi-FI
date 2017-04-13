<properties
   pageTitle="Tekniset vaatimukset Marketplacen tietoja palvelun luontiin | Microsoft Azure"
   description="Tietoja vaatimukset käyttöönotto ja myydä Azure Marketplace-tietojen palvelun luomiseen"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="hascipio; avikova" />

# <a name="technical-pre-requisites-for-creating-a-data-service-offer-for-the-azure-marketplace"></a>Tekniset vaatimukset tietojen palvelun luontiin tarjouksen Azure Marketplacesta

>[AZURE.IMPORTANT] **Tällä hetkellä onboarding olemme eivät ole enää mitään uusi tietopalvelu julkaisijat. Uusi dataservices ei Hae hyväksyä luettelo.** Jos haluat julkaista AppSource SaaS yrityssovelluksen löydät lisätietoja [tähän](https://appsource.microsoft.com/partners). Jos sinulla IaaS sovellusten tai haluat julkaista Azure Marketplace-Kehitystyökalut-palvelun löytyy lisätietoja [tästä](https://azure.microsoft.com/marketplace/programs/certified/).

Lue huolellisesti ennen kuin aloitat prosessin ja ymmärtää, jossa ja miksi jokaisen vaiheen suoritetaan. Mahdollisimman hyvin, sinun olisi yritystietojen ja muiden tietojen valmisteleminen, lataa tarvittavat työkalut ja/tai luo tekniset osat ennen aloittamista tarjouksen luontiprosessi.

Sinulla on oltava valmiina ennen aloittamista prosessin seuraavia kohteita:

## <a name="make-a-decision-on-what-technology-will-be-used-to-publish-your-data-service-offer"></a>Valitse mitä tekniikkaa käytetään julkaista tietopalvelu tarjous päätöksentekoa

Julkaisijan voit valita useita tekniikoita julkaistessasi tietopalvelu Azure Marketplacesta. Tärkeimmät tekniikoita, joita tuetaan seuraavalla tavalla. Riippumatta loppukäyttäjän mitä tekniikkaa käytetään tietojen julkaisemiseen, käyttämän kautta **OData-syötteestä** näyttämiä Azure Marketplace-palvelun tiedot. Täydelliset tiedot OData-palvelun löydät [http://www.odata.org/](http://www.odata.org/)

## <a name="sql-azure-database"></a>SQL Azure-tietokanta

Publisherin vastuu on ottaa tietojoukko valmis SQL Azure-tietokannassa. Sinun on tilaa Azure, tietokannan sopiva koko valmistelu ja lataa tiedot SQL Azure-tietokantaan. Publisherissa on myös kyseistä tiedot pysyvät aina ajan tasalla. Lisätietoja Azure Services tilaaminen löydät [https://azure.microsoft.com/services/sql-database/](https://azure.microsoft.com/services/sql-database/)


Kun siirrät tietojen tuominen SQL Azure-Azure Marketplacesta voit näyttää taulukot ja näkymät. Julkaisija voit määrittää, mitkä taulukot ja näkymät ja sarakkeiden niitä julkaista käyttäjä. Edelleen sisältötoimittajan voit myös määrittää, mitkä sarakkeet voidaan suorittaa kysely loppukäyttäjän ja mitkä ovat vain määrää paketti uudelleen. Näin joustavuutta siitä, mitä tietokanta tarjoamia korkean tason. Sarakkeet, jotka voidaan suorittaa kysely on yksi tai useampi tietokanta indeksien tilannevedosten.

## <a name="rest-based-web-service"></a>Mukaan REST-verkkopalvelu

Tuetut protokolla: **vain HTTPS**

Olemassa olevia REST-pohjaisia palveluja on näkyvät kautta Azure Marketplacesta. Koska dataset aina tarjoamia OData-syötteen kuin käyttäjälle, Azure Marketplace-palvelu on oltava voi yhdistää palvelun muodostaminen OData-pohjainen palvelu. Voit tehdä sen muille käyttäjille perusteella päätepisteet on näyttää kaikki parametrit HTTP parametreiksi.

Tiedot on oltava lomake, joka voidaan yhdistää ATOM-vastauksen. Näin ollen services tarvitsee XML-muotoon ja vain voi olla vastaus sisältää toistuvan elementin, joka sisältää tiedot-arvot (kuten tietuejoukon). Azure Marketplace-palvelun yhdistetään toistuvan solmu tapahtuma-solmu ATOM ja paketti-arvot yhdeksi tapahtuma-solmu sijaitsevien solmujen ominaisuus.

Käyttöoikeustiedot (kuten Ohjelmointirajapinnan avain, todennus käyttöoikeustietue, jne.) on oltava edellyttäen HTTP-parametri tai HTTP-otsikon (avainarvon pari) – perustodentamista tuetaan myös. Kelvollinen avain on annettava ja kaikki pyynnöt kautta Azure Marketplacesta tehdään avaimen avulla. Käyttäjän seuranta ja laskutus tapahtuu Azure Marketplace-tasolla.

Palvelun palauttamat virheet on yhdistetty yhdeksi HTTP-tilakoodin. Siltä varalta, että palvelu palauttaa virheen sisältävä XML nämä sujuvat määrittää HTTP-tila-koodien Azure Marketplace-palvelun.

## <a name="soap-based-web-services"></a>Mukaan SOAP-verkkopalvelut

Protokolla: **vain HTTPS**

Vaatimukset ovat samoja kuin muiden perusteella service-kohtaan. Ainoa ero on, että parametrit voidaan antaa myös XML-tekstissä, joka on lähetetty julkaisijan palveluun kanssa jokaisen pyynnön Azure Marketplacesta. Tämä tarkoittaa, että käyttäjä saa edusta HTTP-parametrit on käännetty tuominen XML-elementtien XML-asiakirjan, joka kirjataan pyynnön sisältötoimittajan WWW-palvelun kanssa.

## <a name="odata-based-web-services"></a>OData-pohjainen web-palvelut

Protokolla: **vain HTTPS**

Tietoja voi tarjoamia OData-palveluna Azure Marketplace. Järjestelmä välittää palvelun kautta suorittaminen ja korvaa palvelun ylimmällä Azure Marketplacesta palvelun pääkansio – varmistaaksesi, että kaikki seuraavat suosikkiryhmän puhelut Siirry Azure Marketplacesta kautta.

OData-palveluiden vain tarvitse Siirry vastaan taustaan tietokannasta. OData tukee kaikenlaista tallennustilan tai liiketoimintalogiikka levyasemassa palvelu.


## <a name="next-steps"></a>Seuraavat vaiheet
Tarkistaa vaatimat ja valmiit tarvittavat tehtävät, voit siirtää eteenpäin luomisessa tietopalvelu tarjous kuin yksityiskohtaiset [Tiedot palvelun julkaisun opas](marketplace-publishing-data-service-creation.md).

Tai jos haluat tarkistaa koko prosessi ja vastaaviin artikkeleissa julkaisun vaiheet, Lisätietoja on artikkelissa [käytön aloittaminen: julkaiseminen tarjouksen Azure Marketplacesta](marketplace-publishing-getting-started.md).

[link-acct]:marketplace-publishing-accounts-creation-registration.md
