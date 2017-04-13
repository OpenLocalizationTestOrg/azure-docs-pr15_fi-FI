<properties
    pageTitle="Azure Active Directory-B2C: Tuotannon-akseli ja Esikatsele B2C alihallintoihin | Microsoft Azure"
    description="Azure Active Directory-B2C alihallinnat tyypit aiheesta"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-production-scale-vs-preview-b2c-tenants"></a>Azure Active Directory-B2C: Tuotannon-akseli ja Esikatsele B2C alihallintoihin

Jos kirjoitat tuotannon app Azure Active Directory (Azure AD) B2C, tarvitset on, että oikea "tyyppi", siirry live-vuokraajan. Voit tarkistaa, miltä muistikirjoihin toimimalla seuraavasti [B2C ominaisuudet-sivu](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) Siirry Azure-portaalissa ja Etsi **vuokraajan tyyppi**.

## <a name="summary"></a>Yhteenveto

Azure AD-B2C tukee vain käytössä **tuotannon asteikko** B2C alihallintoihin Pohjois-Amerikassa tuotannon sovellukset.

| Vuokraajan tyyppi | Palauttaa niiden maiden/alueiden tietueet | Yleensä-käytettävissä? |
| ----------- | -------------- | --------------------- |
| **Tuotannon asteikko vuokraajan** | Pohjois-Amerikan maiden/alueiden tietueet | Kyllä |
| **Tuotannon asteikko vuokraajan** | Kaikkien maiden/alueiden tietueet lukuun ottamatta Pohjois-Amerikka | Ei |
| **Vuokraajan esikatselu** | Kaikkien maiden/alueiden tietueet | Ei |

> [AZURE.NOTE]
(Kuluttajille) Azure AD-B2C alihallinnat, jotka eivät ole käytettävissä muutaman maat tai alueet, jossa (työntekijöille) Azure AD alihallinnat, jotka ovat käytettävissä. Lue lisätietoja seuraavissa osissa.

## <a name="production-scale-b2c-tenant-in-north-america"></a>Tuotannon asteikko B2C vuokraajan Pohjois-Amerikassa

Jos olet [luonut B2C vuokraajan](active-directory-b2c-get-started.md) Pohjois-Amerikassa eli jossakin seuraavista maat tai alueet: United hyötyä, Kanada, Costa Rica, Dominikaaninen tasavalta, El Salvador, Guatemala, Ruotsi, Panama, Puerto Rico ja Trinidad ja Tobago, ja B2C järjestelmänvalvoja-Käyttöliittymän **vuokraajan tyyppi** lukee **tuotannon mittakaava**, alihallintaan voi käyttää tuotannon sovellukset.

> [AZURE.NOTE]
Tuotannon asteikko alihallinnat pystyvät kuluttajien käyttäjätietojen kohden miljoonia 100s skaalaus.

![Näyttökuva tuotannon asteikko palvelutili](./media/active-directory-b2c-reference-tenant-type/production-scale-b2c-tenant.png)

## <a name="preview-b2c-tenant-in-any-countryregion"></a>Mikä tahansa maa/alue-vuokraajan B2C esikatselu

Jos olit luonut B2C palvelutili Azure AD B2C esikatselu aikana, on todennäköistä, että **vuokraajan Kirjoita** lukee **vuokraajan esikatselu**. Jos näin on, sinun on käytettävä alihallintaan kehittäminen ja testauksen ja ei tuotannon sovellukset.

> [AZURE.IMPORTANT]
Ei ole siirtymispolku esikatselusta B2C vuokraajan tuotannon asteikko B2C vuokraajalle. Huomaa, että aiheuttavat tunnetusti ongelmia, kun Poista esikatselu B2C palvelutili ja luo uudelleen tuotannon asteikko B2C palvelutili saman toimialuenimeä. Sinun on luotava tuotannon asteikko B2C palvelutili eri toimialuenimeä.

![Näyttökuva esikatselu palvelutili](./media/active-directory-b2c-reference-tenant-type/preview-b2c-tenant.png)

## <a name="production-scale-b2c-tenant-outside-of-north-america"></a>Tuotannon asteikko B2C vuokraajan Pohjois-Amerikan ulkopuolella

Azure AD-B2C ei tällä hetkellä ole yleensä-käytettävissä Pohjois-Amerikan ulkopuolella. Voit kuitenkin luominen ja käyttäminen tuotannon asteikko-vuokraajiin kehittämiseen tarkoituksiin jollakin seuraavista maat tai alueet: Algeria, Itävalta, Azerbaidžan, Bahrain, Valko-Venäjä, Belgia, Bulgaria, Kroatia, Kypros, tšekki, Tanska, Egypti, Viro, Suomi, ranska, saksa, kreikka, unkari, Islanti, Irlanti, Israel, italia, Jordania, Kazakstan, Kenia, Kuwait, Lativa, Libanon, Liechtenstein, Liettuan, Luxemburg, Makedonia EJTM, Malta, Montenegro, Marokko, Alankomaat, Nigeria, Norja , Oman, Pakistan, puola, portugali, Qatar, Romania, venäjä, Saudi-Arabia, Serbia, Slovakia, Slovenia, Etelä-Afrikka, espanja, Ruotsi, Sveitsi, Tunisia, Turkki, Ukraina, Yhdistyneet Arabiemiirikunnat ja Iso-Britannia.

Kun Azure AD B2C ilmoittaa Yleiset käytettävyys yläpuolella maat tai alueet, voit jatkaa näiden tuotannon asteikko omistajien ja siirry live tuotannon-sovelluksissa menettämättä mitään tietoja.

## <a name="availability-of-b2c-tenants"></a>Käytettävyys B2C alihallintoihin

B2C alihallinnat, jotka eivät ole käytettävissä seuraavat maat tai alueet: Afganistan, Argentiina, Australia, Brasilia, Chile, Kolumbia, Ecuador, Hongkong, Erityishallintoalue, Intia, Indonesia, Irak, japani, Korea, Malesia, Uusi-Seelanti, Paraguay, Peru, Filippiinit, Singapore, Sri Lanka, Taiwan, Thaimaa, Uruguay ja Venezuela. Emme suunnittele sisällyttää ne myöhemmin.
