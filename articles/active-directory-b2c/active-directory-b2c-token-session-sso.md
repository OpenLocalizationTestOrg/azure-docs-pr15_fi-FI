<properties
    pageTitle="Azure Active Directory-B2C: Tunnuksen, istunnon ja Sign-määritys | Microsoft Azure"
    description="Tunnuksen, istunnon ja yksittäisen Sign määrittäminen Azure Active Directory-B2C"
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
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-token-session-and-single-sign-on-configuration"></a>Azure Active Directory-B2C: Tunnuksen, istunnon ja Sign-määritys

Tämä toiminto antaa tarkasti rajattuja ohjausobjektiin [käytäntö peruste](active-directory-b2c-reference-policies.md)
 
1. Suojauksen tunnusten Azure Active Directory (Azure AD) B2C lähettämän käyttöajan.
2. Web-sovelluksen istunnot Azure AD B2C hallitsee käyttöajan.
3. Kertakirjautuminen (SSO) toiminta eri sovellukset ja käytännöt B2C vuokraajan välillä.

Voit käyttää tätä toimintoa B2C vuokraajan seuraavasti:

1. Seuraavasti [B2C ominaisuudet-sivu](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) Siirry Azure-portaalissa.
2. Valitse **Kirjaudu sisään käytännöt**. *Huomautus: Voit käyttää tätä ominaisuutta ei vain tämän käytännön minkä tahansa tyypin* *Kirjaudu sisään käytännöt***.
3. Avaa käytäntö napsauttamalla sitä. Valitse esimerkiksi **B2C_1_SiIn**.
4. Valitse sivu yläreunassa **Muokkaa** .
5. Valitse **tunnuksen, istunnon ja Sign config**.
6. Tee haluamasi muutokset. Tietoja seuraavien osien käytettävissä olevat ominaisuudet.
7. Valitse **OK**.
8. Napsauta **Tallenna** ylälaidassa sivu.

![Näyttökuva tunnuksen, istunnon ja Sign config](./media/active-directory-b2c-token-session-sso/token-session-sso.png)

## <a name="token-lifetimes-configuration"></a>Suojaustunnuksen käyttöajan määritys

Azure AD-B2C tukee [OAuth 2.0 luvan protokolla](active-directory-b2c-reference-protocols.md) suojatun suojatun resurssien käytön aloittamiseen. Toteuttamaan tuki Azure AD B2C tietokoneesta, kuuluu äänimerkki on eri [suojauksen tunnukset](active-directory-b2c-reference-tokens.md). Voit hallita suojauksen tunnusten Azure AD B2C lähettämän käyttöajan ominaisuudet ovat seuraavat:

- **Access & AS-tunnuksen elinaika (minuutteina)**: OAuth 2.0-haltijatunnukseen elinkaaren käyttää suojatun resurssin käyttöoikeutta. Azure AD-B2C ongelmat tällä hetkellä vain tunnus tunnukset. Tämä arvo koskee myös access-tunnusten lisäämme niihin tuki.
   - Oletus = 60 minuuttia.
   - Pienin (mukaan lukien) = 5 minuuttia.
   - Suurin (mukaan lukien) = 1 440 minuuttia.
- **Päivityksen suojaustunnuksen elinaika (päiviä)**: suurin ajanjakso, jota ennen päivityksen tunnuksen avulla voidaan hankkia uuden access- tai tunnus (ja halutessasi uuden päivityksen suojaustunnuksen, jos sovellus oli myönnetty `offline_access` laajuus).
   - Oletus = 14 päivän ajan.
   - Pienin (mukaan lukien) = 1 päivä.
   - Suurin (mukaan lukien) = 90 päivää.
- **Päivityksen tunnuksen liukuva ikkunan elinaika (päiviä)**: kun tänä kuluu käyttäjä on pakko todennetaan uudelleen, riippumatta voimassaoloajan viimeisimmän Päivitä sovellus hankittu tunnuksen. Se saadaan vain, jos valitsin on määritetty **Bounded**. Se on oltava suurempi tai yhtä suuri kuin **päivityksen suojaustunnuksen elinaika (päiviä)** arvo. Jos Vaihda on määritetty **Unbounded**, ei voi antaa tietyn arvon.
   - Oletus = 90 päivää.
   - Pienin (mukaan lukien) = 1 päivä.
   - Suurin (mukaan lukien) = 365 päivää.

Nämä ovat pari Käytä tapauksissa, voit ottaa käyttöön näiden ominaisuuksien avulla:

- Salli käyttäjille pysyt jatkuvasti, allekirjoitetun mobiili-sovellukseen, kun hän on jatkuvasti aktiivinen sovellus. Voit tehdä tämän määrittämällä **päivityksen suojaustunnuksen liukuva ikkunan elinaika (päiviä)** Siirry **Unbounded** kirjautumisen käytännön.
- Täyttävät oman alan suojaus ja yhteensopivuusvaatimukset määrittämällä käyttöoikeuksien suojaustunnuksen käyttöajan.

## <a name="session-configuration"></a>Istunnon määritys

Azure AD-B2C tukee [OpenID yhteyden protokollaa](active-directory-b2c-reference-oidc.md) suojattu kirjautuminen web-sovellusten käyttöön. Voit hallita web-sovelluksen istunnot ominaisuudet ovat seuraavat:

- **Web-sovelluksen istunnon elinaika (minuutteina)**: Azure AD B2C istunnon eväste tallennetun käyttäjän selaimen yhteydessä todentaminen onnistuu elinkaaren.
   - Oletus = 1 440 minuuttia.
   - Pienin (mukaan lukien) = 15 minuuttia.
   - Suurin (mukaan lukien) = 1 440 minuuttia.
- **Web app-istunnon aikakatkaisu**: tätä valitsinta on määritetty päivittymään **suorien**, jos käyttäjä on pakotettu tarkistamiseen uudelleen **Web Appin istunnon elinaika (minuutteina)** kuluu määräajan jälkeen. Jos tätä valitsinta on määritetty **liikkuva** (oletusasetus), käyttäjä pysyy kirjautuneena, kun käyttäjä on aktiivisena jatkuvasti web-sovelluksen.

Nämä ovat pari Käytä tapauksissa, voit ottaa käyttöön näiden ominaisuuksien avulla:

- Täytä oman alan tietosuojaan ja vaatimustenmukaisuuteen vaatimuksia määrittämällä haluamasi web-sovelluksen istunnon käyttöajan.
- Voit pakottaa uudelleen todennusta määritetyn ajan kuluttua käyttäjän web-sovelluksen osa-suojauksen käsittelemisen aikana. 

## <a name="single-sign-on-sso-configuration"></a>Kertakirjautuminen (SSO) määrittäminen

Jos sinulla on useita sovelluksia ja käytännöistä B2C vuokraajan, voit hallita käyttäjän vuorovaikutukset kattavia **Sign-määritys** -ominaisuuden avulla. Voit määrittää ominaisuuden jokin seuraavista asetuksista:

- **Vuokraajan**: Tämä on oletusarvo. Tämän asetuksen avulla useita sovelluksia ja käytännöistä B2C vuokraajan jakaa saman käyttäjäistunnon. Esimerkiksi kun käyttäjä kirjautuu sovelluksen, Contoso ostokset, hän voi myös saumattomasti kirjautua toiseen kommentin Contoso Apteekin, yhteydessä käyttää sitä.
- **Sovelluksen**: Tässä voit säilyttää käyttäjäistunnon yksinomaan sovelluksen erillään muista sovelluksista. Esimerkiksi jos haluat käyttäjän kirjautuminen Contoso Apteekin (avulla samoja tunnistetietoja), vaikka hän on jo kirjautunut sisään Contoso ostokset, toisessa sovelluksessa samaan B2C vuokraaja. 
- **Käytäntö**: Tässä voit säilyttää käyttäjäistunnon yksinomaan käytännön, käyttää sitä sovellusten riippumatta. Esimerkiksi jos käyttäjällä on jo kirjautunut sisään ja usean kerroin todentaminen (MFA)-vaiheen, hän voidaan kirjoittaa access-suojauksen osiin useita sovelluksia, kunhan sidottu käytännön istunnon ei vanhene.
- **Ei käytössä**: Tämä pakottaa käyttäjä voi suorittaa läpi koko käyttäjän matkan käytännön jokaisen suorittamisen. Esimerkiksi Tämä sallii useiden käyttäjien kirjautua (Valitse jaetun työpöydän tilanne)-sovellukseen, parillinen aikana yksittäisen käyttäjän pysyy kirjautuneena koko ajan kuluessa.
