<properties
    pageTitle="Azure pinon avaimen säilö johdanto | Microsoft Azure"
    description="Lue, miten Azure pinon avaimen säilö hallitsee näppäimet ja tietoja"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="introduction-to-key-vault-in-azure-stack"></a>Johdanto Azure Pinotut avaimen säilö #

## <a name="before-you-start"></a>Ennen aloittamista

Tässä artikkelissa oletetaan seuraavasti:

- Lukija on tilauksessa, joka on käytössä Azure avaimen säilö.
- Azure PowerShell SDK on määritetty ja käytettävissä.
- Kaikki vuokraajan osoittava toimintoja voi käyttää vain PowerShell-TP2-versiossa.

## <a name="key-vault-basics"></a>Avaimen säilö perusteet

Avaimen säilö Azure Pinotut auttaa suojaa salausavaimet ja käyttää tietoja, jotka cloud sovelluksia ja palveluja. Avaimen säilö käyttämällä voit salata näppäimet ja tietoja (kuten todennus näppäimet, tallennustilan tilin näppäimet, tietojen salausavaimet, .pfx-tiedostot ja salasanat).

Avaimen säilö hallintaa tehostava ja avulla voit hallita näppäinyhdistelmien, joka käyttää ja tietojen salaamiseen. Kehittäjien luoda näppäimet kehittäminen ja testaus minuutteina, ja siirrä niitä saumattomasti tuotannon avaimet. Suojauksen järjestelmänvalvojat voivat myöntää (ja kumota) oikeuden näppäimet-tarpeen mukaan.

Kuka tahansa käyttäjä, Azure pino-tilauksella voit luoda ja käyttää avaimen vaults. Vaikka avain säilö hyötyä kehittäjille ja suojaus-järjestelmänvalvojille, se voidaan toteuttaa ja hallitsee järjestelmänvalvoja, joka hallinnoi Azure pinon muiden organisaation. Esimerkiksi tämän järjestelmänvalvojan kirjautua Azure pinon tilauksen luominen säilöön, jonne haluat tallentaa näppäimet ja sitten vastuussa toiminnallisia tehtäviä organisaation:

- Luo tai tuo avaimen tai salaisuus
- Poistaminen tai salaisuus tai avaimen poistaminen
- Määritä käyttäjät tai sovellukset voivat käyttää avaimen säilö, jotta he voivat hallita tai sen näppäimet ja tietoja
- Määritä avaimen käyttö (esimerkiksi allekirjoittaa tai salata)

Tämän järjestelmänvalvoja voi sitten antaa kehittäjät URI soittaessasi sovellukset ja antaa suojauksen järjestelmänvalvojan avaimen käyttö lokiin kirjaaminen.

Kehittäjät voivat myös hallita näppäimet suoraan käyttämällä API. Saat lisätietoja avaimen säilö kehittäjän oppaassa.

## <a name="scenarios"></a>Skenaariot

Seuraavassa taulukossa kuvataan joitakin skenaariot, jossa avaimen säilö voivat auttaa tarpeiden mukaan kehittäjille ja suojaus-järjestelmänvalvojat:


### <a name="developer-for-an-azure-stack-application"></a>Azure-pino sovelluksen Developer

**Ongelma**: halua kirjoittaa sovelluksen Azure-pino, joka käyttää näppäimet allekirjoituksen ja salauksen, mutta haluan, että ne on ulkoisen sovelluksen lähettämät, jotta ratkaisu sopii sovellus, joka on jaettu maantieteellisesti.

**Lause**: näppäimet säilöön tallennettuja ja käynnistää URI tarpeen mukaan.


### <a name="developer-for-software-as-a-service-saas"></a>Kehitystyökalut-ohjelmiston serviceksi (hyväksymislautakunnan)

**Ongelma:** En halua vastuu tai mahdollisten vastuuta Omat asiakkaat vuokraajan näppäimet ja tietoja.

**Lause:** Asiakkaat voivat tuoda oman näppäimet Azure pinon ja hallita niitä. Haluan asiakkaita omia ja hallita niiden näppäimet niin, että voit keskittyä tavoilla, mitä voin tehdä parhaiten, joka tarjoaa tärkeä ohjelmiston ominaisuuksia.


### <a name="chief-security-officer-cso"></a>Johtava viranomainen (yhtiön)

**Ongelma:** Haluan, että organisaation hallitsee avaimen elinkaaren ja valvoa avaimen käyttö.

**Lause** Avaimen säilö on suunniteltu siten, että Microsoft ei tarkastella tai Pura avaimien.  Kun sovellus täytyy suorittaa salauksen asiakkaiden näppäimiä käyttämällä, avaimen säilö tekee tämän sovelluksen puolesta. Sovellus ei näy niiden asiakkaiden avaimet.  Käytämme useita Azure pinon palvelut ja resurssit, mutta haluan hallita näppäimet Azure Pinotut yhdestä paikasta. Säilö on käyttöliittymä, riippumatta siitä, kuinka monta vaults käytössä Azure Pinotut, mitkä alueet ne tukeen ja mitkä sovellukset käyttää niitä.

## <a name="next-steps"></a>Seuraavat vaiheet

[Avaimen säilö käytön aloittaminen](azure-stack-kv-getting-started.md)
