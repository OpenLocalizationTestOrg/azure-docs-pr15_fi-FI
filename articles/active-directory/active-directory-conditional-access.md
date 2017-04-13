<Properties
    pageTitle="Azure Active Directory ehdollisen käyttöoikeuden | Microsoft Azure"  
    description="Azure Active Directory ehdollinen käytönvalvonta avulla voit tarkistaa tiettyjä ehtoja todennuksen sovellusten käytön yhteydessä."  
    services="active-directory"
    keywords="Ehdollinen pääsy sovellukset, ehdollinen Azure AD-käytön, yrityksen resurssien ehdollisen käyttöoikeuden käytännöt suojattu käyttö"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/21/2016"
    ms.author="markvi"/>


# <a name="conditional-access-in-azure-active-directory"></a>Azure Active Directoryn ehdollisen käyttöoikeuden   

Ohjausobjektin ominaisuuksia Azure Active Directory (Azure AD) ehdollinen Accessissa tarjouksen yksinkertaista tapaa, joilla cloud ja paikallisen suojatun resurssit. Ehdollinen käytäntöjen kuin monimenetelmäisen todentamisen suojautua riskiä varastamiselta ja phished tunnistetiedot. Muut ehdollinen käytäntöjen avulla lukemaan organisaation tiedot. Esimerkiksi lisäksi edellyttävän tunnistetiedot, sinun on ehkä käytännön, vain laitteissa, jotka kuuluvat mobiililaitteen hallintajärjestelmän, esimerkiksi Microsoft Intune voit käyttää organisaation luottamuksellisia palveluja.


## <a name="prerequisites"></a>Edellytykset

Azure AD ehdollisen käyttöoikeuden toimintoa [Azure Active Directory-Premium](http://www.microsoft.com/identity). Sovellus, jossa on käytetty ehdollista access käytännöt kullekin käyttäjälle on oltava Azure AD Premium-käyttöoikeus. Voit lukea lisää käyttöoikeusvaatimukset- [Käyttöoikeudettomien käyttäjien raportti](https://aka.ms/utc5ix).


## <a name="how-is-conditional-access-control-enforced"></a>Miten ehdollinen käytönvalvonta on käytössä?  

Ehdollinen access-komponentin paikassa Azure AD tarkistaa, tietyt ehdot käyttäjä voi käyttää sovelluksen määrittäminen. Access-vaatimukset täyttyvät, kun käyttäjä todennetaan ja voit käyttää sovellusta.  

![Ehdollisen käyttöoikeuden yleiskatsaus](./media/active-directory-conditional-access/conditionalaccess-overview.png)

## <a name="conditions"></a>Ehdot

Seuraavassa on kuvattu tilanteita, joissa voit sisällyttää ehdollinen Käyttöoikeuskäytäntö:

- **Ryhmäjäsenyyden**. Hallita ryhmän jäsenyyden perusteella käyttäjän.

- **Sijainti**. Käytä käynnistimen monimenetelmäisen todentamisen käyttäjän sijainti ja käyttää estä ohjausobjekteja, kun käyttäjä ei ole luotetussa verkossa.

- **Laitteen ympäristössä**. Määritä laitteen alustan, kuten iOS-, Android-, Windows Mobile- ja Windows-ehdon käytäntö otetaan käyttöön.

- **Laite on otettu käyttöön**. Laitteen tila, onko käytössä tai poissa käytöstä, tarkistetaan laitteen käytännön arvioinnin aikana. Jos poistat kadonneiden tai sitä laitteen hakemistossa, se ei enää täytä käytännön vaatimuksia.

- **Kirjaudu sisään- ja käyttäjän riski**. Voit käyttää Ehdollinen riskin käytäntöjen [Azure AD-tunnistetiedot](active-directory-identityprotection.md) . Ehdollinen riskin käytäntöjen lukemalla organisaation advance suojausta riskin tapahtumien ja epätavallisia Kirjautumisvirheitä aiheuttavien toimintojen perusteella.


## <a name="controls"></a>Ohjausobjektit

Seuraavassa on kuvattu ohjausobjektit, jonka avulla voidaan valvoa ehdollinen Käyttöoikeuskäytäntö:

- **Monimenetelmäisen todentamisen**. Voit määrittää vahva todennusta ja monimenetelmäisen todentamisen. Voit käyttää monimenetelmäisen todentamisen Azure Monimenetelmäisen todentamisen tai käyttämällä paikallisen monimenetelmäisen todentamisen tarjoajan, yhdistää Active Directory Federation Services (AD FS). Monimenetelmäisen todentamisen käyttäminen auttaa suojaamaan resurssien käytössä luvaton käyttäjä, joka saattaa on pääsy kelvollinen käyttäjän tunnistetiedot.

- **Estä**. Voit lisätä ehtoja, kuten käyttäjän sijainti käytön estäminen. Voit esimerkiksi estää access, kun käyttäjä ei ole luotetussa verkossa.

- **Yhteensopivat laitteet**. Voit määrittää ehdollisen käyttöoikeuden käytännöt laitteen tasolla. Voit määrittää käytännön niin, että vain tietokoneissa, joissa on toimialueeseen liittymistä tai mobiililaitteiden, joka on rekisteröity mobiililaitteiden hallinta-sovellusta voi käyttää organisaation resursseja. Voit esimerkiksi Intune avulla voit tarkistaa laitteen vaatimustenmukaisuus ja raportoida asiasta Azure Active Directory käyttäminen, kun käyttäjä yrittää käyttää sovelluksen. Yksityiskohtaisia ohjeita Intune käyttämisestä sovellukset ja tietojen suojaamiseen on artikkelissa [Suojaa sovellukset ja Microsoft Intune-tietojasi](https://docs.microsoft.com/intune/deploy-use/protect-apps-and-data-with-microsoft-intune). Intune avulla voit myös pakottaa kadonneiden tai sitä laitteiden tietojen suojauksen. Lisätietoja [auttaa suojaamaan tietosi täyden ja valikoivan tietojen poiston käyttämällä Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune).

## <a name="applications"></a>Sovellukset

Voit pakottaa ehdollinen käyttöoikeuskäytäntö sovelluksen tasolla. Määrittää käyttöoikeuksia sovellusten ja palveluiden cloud tai paikalliseen. Käytäntö otetaan käyttöön suoraan sivustoon tai palveluun. Käytäntö on käytössä selaimessa ja sovelluksia, jotka käyttävät palvelun käyttöoikeus.


## <a name="device-based-conditional-access"></a>Laitteen perustuva ehdollisen käyttöoikeuden

Voit rajoittaa sovellusten laitteilla, joka on rekisteröity Azure AD ja täyttävät tietyt ehdot. Laitteen perustuva ehdollisen käyttöoikeuden suojaa organisaation resurssien käyttäjät, jotka yrittävät tarkastella resurssit:

- Tuntematon tai ei-hallitun laitteet.
- Laitteet, jotka eivät täytä suojauskäytäntöjen määrittäminen organisaation.

Voit määrittää käytäntöjä näitä vaatimuksia perusteella:

- **Toimialueen liittynyt laitteet**. Määrittää käytännön, rajoittaaksesi laitteet, jotka ovat liittyneet paikallisen Active Directory-toimialueen ja myös tunnistettujen Azure AD kanssa. Käytäntö koskee Windows työpöytiä, kannettavia tietokoneita ja yrityksen tablettien.
Saat lisätietoja automaattisen rekisteröinnin Azure AD toimialueen liittynyt laitteiden määrittämisestä [määrittäminen automaattinen rekisteröinti Windows Azure Active Directory toimialueen liittynyt laitteiden](active-directory-conditional-access-automatic-device-registration-setup.md).

- **Yhteensopivat laitteet**. Määrittää käytännön, rajoittamisesta laitteita, jotka on merkitty **yhteensopiva** hallinta järjestelmän hakemistossa. Tämän käytännön varmistaa, että vain laitteita, jotka täyttävät kuten pakotetut tiedoston salauksen laitteen suojauskäytännöt sallitaan access. Voit käyttää tämän käytännön käytön rajoittaminen seuraavissa laitteissa:

    - **Windows-toimialueeseen liittyneet laitteet**. Hallitun mukaan System Center määritysten hallinta (Valitse nykyinen haara) hybrid määrityksessä käyttöön.
    - **Windows 10 Mobile työ- tai henkilökohtaisten laitteiden**. Hallita Intune tai kolmannen osapuolen tuetut mobiililaitteiden hallintajärjestelmän.
    - **iOS- ja Android-laitteisiin**. Hallitsee Intune.


Sovellukset, jotka on suojattu laite-pohjaisen käyttävien käyttäjien myöntäjän käytäntö on käyttää sovelluksen laitteesta, joka vastaa tämän käytännön. Käyttö on estetty yritti laitteessa, joka ei täytä käytännön vaatimuksia.

Lisätietoja laitteen perustuvan varmenteiden myöntäjä-käytännön määrittäminen Azure AD-kohdassa [laitteen perustuva ehdollisen käyttöoikeuden käytännön sovellusten Azure Active Directory-yhteys](active-directory-conditional-access-policy-connected-applications.md).

## <a name="resources"></a>Resurssit

Katso seuraavat resurssin luokat ja artikkeleita lisätietoja määrittäminen ehdollisen käyttöoikeuden organisaatiollesi.


### <a name="multi-factor-authentication-and-location-policies"></a>Multi-factor authentication ja sijainnin määrittäminen

- [Azure AD-yhteys sovelluksiin ryhmittely, sijainti ja monimenetelmäisen todentamisen määrittäminen perustuu ehdollinen käytön aloittaminen](active-directory-conditional-access-azuread-connected-apps.md)
- [Sovellukset, joita tuetaan](active-directory-conditional-access-supported-apps.md)


### <a name="device-based-conditional-access"></a>Laitteen perustuva ehdollisen käyttöoikeuden

- [Azure Active Directory-yhteys sovellusten käyttöoikeuksien hallinta käytännön laitteen perustuva ehdollisen käyttöoikeuden asettaminen](active-directory-conditional-access-policy-connected-applications.md)
- [Määrittäminen automaattinen rekisteröinti Windows Azure Active Directory-hakemistosta toimialueeseen liittyneet laitteet](active-directory-conditional-access-automatic-device-registration-setup.md)
- [Azure Active Directory access-ongelmien vianmääritys](active-directory-conditional-access-device-remediation.md)
- [Suojata tietoja kadonneiden tai sitä laitteissa Microsoft Intune avulla](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune)


### <a name="protect-resources-based-on-sign-in-risk"></a>Suojaa resurssit-kirjautuminen riskin perusteella

-   [Azure AD-tunnistetietojen suojaus](active-directory-identityprotection.md)

### <a name="next-steps"></a>Seuraavat vaiheet

- [Ehdollisen käyttöoikeuden usein kysyttyjä kysymyksiä](active-directory-conditional-faqs.md)
- [Tekniset tiedot](active-directory-conditional-access-technical-reference.md)
