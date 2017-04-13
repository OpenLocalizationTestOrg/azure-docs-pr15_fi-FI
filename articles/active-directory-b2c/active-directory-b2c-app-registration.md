<properties
    pageTitle="Azure Active Directory-B2C: Sovelluksen rekisteröinti | Microsoft Azure"
    description="Sovelluksen rekisteröitymisestä Azure Active Directory-B2C"
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
    ms.topic="get-started-article"
    ms.date="08/30/2016"
    ms.author="swkrish"/>


# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory-B2C: Rekisteröi sovelluksen

## <a name="prerequisite"></a>Edellytyksenä

Luonnissa sovellus, joka hyväksyy kuluttaja kirjautumista ja kirjaudu sisään ensin haluat rekisteröidä sovelluksen Azure Active Directory-B2C vuokraajan. Pyydä oman vuokraajan luominen [Azure AD B2C vuokraajan](active-directory-b2c-get-started.md)kuvattujen vaiheiden avulla. Kun olet suorittanut kaikki artikkelin vaiheet, sinun on kiinnitetty oman Startboard B2C ominaisuudet-sivu.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="navigate-to-the-b2c-features-blade"></a>Siirry B2C ominaisuudet-sivu

Jos sinulla on kiinnitetty oman Startboard B2C ominaisuudet-sivu, sivu tulee näkyviin heti, kun kirjautuminen [Azure portal](https://portal.azure.com/) B2C vuokraajan yleisenä järjestelmänvalvojana.

Voit myös käyttää sivu valitsemalla **Selaa** ja valitse sitten **Azure AD B2C** [Azure portaalin](https://portal.azure.com/)vasemmanpuoleisessa siirtymisruudussa.

> [AZURE.IMPORTANT] Sinun on oltava Yleinen järjestelmänvalvoja, voit käyttää B2C ominaisuudet-sivu B2C-vuokraajan. Muut vuokraajasta Yleinen järjestelmänvalvoja tai minkä tahansa vuokraajasta käyttäjä voi tarkastella sisältöä.  Voit liikkua B2C asiakasympäristöön käyttämällä vuokraajan Vaihtajan Azure-portaalin oikeassa yläkulmassa.

## <a name="register-an-application"></a>Sovelluksen rekisteröiminen

1. Valitse Azure-portaalissa B2C ominaisuudet-sivu Valitse **sovellukset**.
2. Valitse **+ Lisää** yläreunaan sivu.
3. Kirjoita **nimi** sovellus, jossa kerrotaan, että sovelluksesi käyttäjät. Voit kirjoittaa esimerkiksi "Contoso B2C sovelluksen".
4. Jos kirjoitat web-sovellukseen, Vaihda **Sisällytä online / verkko-Ohjelmointirajapinnan** Vaihda arvoksi **Kyllä**. **Vastaa URL-osoitteet** ovat päätepisteet, jossa Azure AD B2C palauttavat tunnuksia, joka pyytää sovelluksen. Kirjoita esimerkiksi `https://localhost:44321/`. Jos web-sovelluksen myös puhelut joitakin verkko-Ohjelmointirajapinnan Azure AD B2C suojattu, kannattaa luoda **Sovelluksen toiminta** myös valitsemalla **Luo avain** -painiketta.

    > [AZURE.NOTE] **Sovelluksen toiminta** on tärkeän tunnistetiedon ja suojata asianmukaisesti.

5. Jos kirjoitat mobile-sovelluksen, Vaihda **Sisällytä native client** Vaihda arvoksi **Kyllä**. Kopioi alaspäin oletusarvo **Uudelleenohjata URI** , jotka luodaan automaattisesti puolestasi.
6. Valitse **Luo** rekisteröidä sovelluksen.
7. Valitse juuri luomasi sovellus ja kopioi GUID **Asiakkaan tunnus** , joka tulee käyttää myöhemmin koodissa alaspäin.

> [AZURE.IMPORTANT] Sovellukset, jotka on luotu B2C ominaisuudet-sivu on hallitun samassa sijainnissa. Jos muokkaat B2C sovellusten PowerShell tai toisen portaalin, ne muuttuvat ei tueta ja todennäköisesti ei toimi Azure AD B2C.

## <a name="build-a-quick-start-application"></a>Pika-aloitusopas-sovelluksen luominen

Nyt kun olet luonut Azure AD B2C rekisteröityjä sovelluksen, voit täyttää Microsoftin quick start opetusohjelmat pääset hyvin alkuun. Seuraavassa on muutamia suositukset:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]
