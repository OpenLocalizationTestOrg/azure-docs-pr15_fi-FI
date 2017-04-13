<properties
    pageTitle="Mikä on Azure avaimen säilö? | Microsoft Azure"
    description="Azure avaimen säilö auttaa suojaavat salausavaimet ja cloud-sovellusten ja palveluiden käyttämän tietoja. Käyttämällä Azure avaimen säilö asiakkaiden voit salata näppäimet ja tietoja (kuten todennus näppäimet tallennustilan tilin näppäimet, tietojen salausavaimet. PFX-tiedostot ja salasanat), joka on suojattu laitteiston suojauksen moduulit (HSMs) näppäimiä käyttämällä."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="cabailey"/>



# <a name="what-is-azure-key-vault"></a>Mikä on Azure avaimen säilö?

Azure avaimen säilö on käytettävissä useimmilla alueilla. Lisätietoja on artikkelissa [avaimen säilö hinnat sivun](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Johdanto

Azure avaimen säilö auttaa suojaavat salausavaimet ja cloud-sovellusten ja palveluiden käyttämän tietoja. Avaimen säilö käyttämällä voit salata näppäimet ja tietoja (kuten todennus näppäimet tallennustilan tilin näppäimet, tietojen salausavaimet. PFX-tiedostot ja salasanat), joka on suojattu laitteiston suojauksen moduulit (HSMs) näppäimiä käyttämällä. Lisätty assurance voit tuoda tai luo näppäimet HSMs. Jos haluat tehdä tämän Microsoft prosessit avaimien FIPS 140-2 tason 2 vahvistettu HSMs (laitteiston ja ohjelmiston).  

Avaimen säilö hallintaa tehostava ja avulla voit hallita näppäinyhdistelmien, joka käyttää ja tietojen salaamiseen. Kehittäjien luoda näppäimet kehittäminen ja testaus minuutteina, ja siirrä niitä saumattomasti tuotannon avaimet. Suojauksen järjestelmänvalvojat voivat myöntää (ja kumota) oikeuden näppäimet-tarpeen mukaan.

Käytä paremmin seuraavan taulukon ymmärtää, kuinka avaimen säilö auttaa tarpeiden mukaan kehittäjille ja suojaus-järjestelmänvalvojille.





| Rooli        | Ongelman kuvaus           | Selviä Azure avaimen säilö  |
| ------------- |-------------|-----|
| Azure-sovelluksen Developer      | "Haluan kirjoittaa sovelluksen Azure, joka käyttää näppäimet allekirjoituksen ja salauksen, mutta haluan painettavat näppäimet on ulkoisen sovelluksen lähettämät, jotta ratkaisu sopii sovellus, joka on jaettu maantieteellisesti. <br/><br/>Haluan myös näppäimet näitä tietoja voi suojata, eikä sinun tarvitse kirjoittaa koodin itse. Myös haluan näppäimet näitä tietoja on helppo minun käyttää Omat sovellukset-parhaan mahdollisen suorituskyvyn." | √ Näppäimet säilöön tallennettuja ja käynnistää URI tarpeen mukaan.<br/><br/> √ Näppäimet turvataan Azure-yleisesti algoritmeista, avaimen ja laitteiston suojauksen moduulit (HSMs) mukaan.<br/><br/> √ Avaimet käsitellään HSMs, jotka asuvat samassa Azure palvelinkeskusten sovelluksina. Tämä on luotettavuutta ja rajoitettu viive kuin jos näppäimet ovat eri sijainnissa, kuten paikallisen.|
| Kehitystyökalut-ohjelmiston serviceksi (hyväksymislautakunnan)      |"En halua vastuu tai mahdollisten vastuuseen Omat asiakkaat vuokraajan avaimet ja tietoja. <br/><br/>Haluan asiakkaita omia ja hallita niiden näppäimet niin, että voit keskittyä tavoilla, mitä voin tehdä parhaiten, joka tarjoaa tärkeä ohjelmiston ominaisuuksia,." | √ Asiakkaat voivat tuoda Omat näppäimet Azure ja hallita niitä. Kun SaaS sovellus täytyy suorittaa salauksen asiakkaidensa näppäimiä käyttämällä, avaimen säilö tekee nämä toiminnot sovelluksen puolesta. Sovellus ei näy niiden asiakkaiden avaimet.|
| Johtava viranomainen (yhtiön) | "Haluan tietää, että Microsoftin sovellusten noudattavat FIPS 140-2 tason 2 HSMs suojatun hallintaa varten. <br/><br/>Haluan, että organisaation hallitsee avaimen elinkaaren ja valvoa avaimen käyttö. <br/><br/>Ja Käytämme useita Azure palvelut ja resurssit, mutta haluan hallita näppäimet yhdestä paikasta Azure-."     |√ HSMs ovat FIPS 140-2 tason 2 vahvistettu.<br/><br/>√ Avaimen säilö on suunniteltu siten, että Microsoft ei tarkastella tai Pura avaimien.<br/><br/>√ Lähellä avaimen käyttö reaaliaikainen kirjaaminen.<br/><br/>√ säilö on käyttöliittymä, riippumatta siitä, kuinka monta vaults käytössä Azure-tietokannassa, mitkä alueet ne tukeen ja mitkä sovellukset käyttää niitä. |


Kuka tahansa käyttäjä, Azure-tilauksella voit luoda ja käyttää avaimen vaults. Vaikka avain säilö hyötyä kehittäjille ja suojaus-järjestelmänvalvojille, se voi toteuttaa ja hallitsee organisaation järjestelmänvalvoja, joka hallinnoi Azure muiden organisaation. Esimerkiksi tämän järjestelmänvalvojan kirjautuminen Azure-tilauksella, luoda säilöön, jonne haluat tallentaa näppäimet ja sitten vastuussa toiminnallisia tehtäviä, kuten organisaation:

+ Luo tai tuo avaimen tai salaisuus
+ Poistaminen tai salaisuus tai avaimen poistaminen
+ Määritä käyttäjät tai sovellukset voivat käyttää avaimen säilö, jotta he voivat hallita tai sen näppäimet ja tietoja
+ Määritä avaimen käyttö (esimerkiksi allekirjoittaa tai salata)
+ Näytön avaimen käyttö

Tämän järjestelmänvalvojan tapaan sitten antaa kehittäjät URI soittaessasi sovellukset ja antaa suojauksen järjestelmänvalvoja avaimen käyttö lokiin kirjaaminen. 

   ![Yleistä Azure avaimen säilö][1]

Kehittäjät voivat myös hallita näppäimet suoraan käyttämällä API. Saat lisätietoja [avaimen säilö kehittäjän](key-vault-developers-guide.md)oppaassa.

## <a name="next-steps"></a>Seuraavat vaiheet

Hakeminen Aloittaminen opetusohjelman järjestelmänvalvoja Katso [Azure avaimen säilö käytön aloittaminen](key-vault-get-started.md).

Saat lisätietoja avaimen säilö, käyttötietojen [Azure avaimen säilö kirjaaminen](key-vault-logging.md).

Lisätietoja näppäimet ja tietoja käyttäminen Azure avaimen säilö kohdassa [tietoja näppäimet, tietoja, ja varmenteet](https://msdn.microsoft.com/library/azure/dn903623\(v=azure.1\).aspx).


<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
