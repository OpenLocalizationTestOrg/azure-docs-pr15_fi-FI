<properties
    pageTitle="Käyttötapoja ja käyttöönottoon liittyviä huomioita Azure AD liittyä | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan, miten järjestelmänvalvojat voivat määrittää määrittäminen Azure AD liittyä niiden käyttäjien (työntekijät, opiskelijoille, muut käyttäjät). Raportissa käsitellään myös eri tosielämän skenaarioiden Azure AD liittyminen käyttämällä."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

< tunnisteet ms.service= "active directory-ms.workload="identity "ms.tgt_pltfrm="na "ms.devlang="na "ms.topic="article "ms.date="09/27/2016 "

    ms.author="femila"/>

# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a>Käyttötapoja ja käyttöönottoon liittyviä huomioita Azure AD liittyminen

## <a name="usage-scenarios-for-azure-ad-join"></a>Käyttö tilanteita, joissa Azure AD liittyminen
### <a name="scenario-1-businesses-largely-in-the-cloud"></a>Tapaus 1: Yritysten suurelta pilvipalvelussa

Azure Active Directory Join (Azure AD-liittyä) voit hyötyä voit, jos tällä hetkellä toimivat ja jäsenyyksiä hallitaan yrityksesi pilveen tai siirrät pilveen pian. Voit käyttää tilille, jolla olet luonut Azure AD Windows 10: een kirjautuminen. [Ensimmäisen Suorita kokemus (FRX)](active-directory-azureadjoin-user-frx.md)prosessin aikana tai yhdistämällä Azure AD- [Asetukset-valikosta](active-directory-azureadjoin-user-upgrade.md)että käyttäjät voivat liittyä niiden koneet Azure AD.  Käyttäjien myös pääset hyödyntämään yhden kertakirjautumisportaaliin (SSO) cloud resurssien käytön Office 365: ssä, kuten selaimessa tai Office-sovelluksissa.

### <a name="scenario-2-educational-institutions"></a>Tapaus 2: Oppilaitosten

Oppilaitosten on yleensä käyttäjän kahdenlaisia: opettajille ja opiskelijoille. Henkilökunnan jäsenten pidetään organisaation kahdeksalla jäsenet. Paikallisen-tilien luominen niiden on suositeltavaa. Mutta opiskelijoiden ovat organisaation shorter-term jäsenet ja niiden tilejä voidaan hallita Azure AD. Tämä tarkoittaa, että kansion asteikko miten pilveen sijaan, että tallennetaan paikallisesti. Se tarkoittaa sitä, että opiskelijat voivat niiden Azure AD-tilien kanssa Windows kirjautuminen ja Office 365-resursseista saat Office-sovelluksissa.

### <a name="scenario-3-retail-businesses"></a>Tapaus 3: Jälleenmyynti yrityksille

Jälleenmyynti yritysten on kausiluonteisista työntekijöiden ja pitkään työntekijöiden. Yleensä paikallisen tilien luominen ja käyttää toimialueen liittyneet koneet kahdeksalla Täysipäiväisen työntekijöille. Kausiluonteisista työntekijät ovat organisaation shorter-term jäsenet mutta kannattaa hallita niiden missä käyttöoikeuksia voidaan helpottaa siirtää ympärille. Kun luot niiden käyttäjätilien pilveen Office 365-käyttöoikeudet, nämä käyttäjät edut kirjautumisessa Windows ja Office sovellusten Azure AD-tiliä, kun niiden käyttöoikeudet joustavammin säilyttää sen jälkeen, kun he lähtevät.

### <a name="scenario-4-additional-scenarios"></a>Tapaus 4: Lisää skenaarioita

Edellä kerrottiin lisäetuja sekä hyötyvät ottaa laitteensa liittäminen Azure AD vuoksi yksinkertaistettu liittävä kokemusta tehokas hallinta, automaattinen mobiililaitteiden hallinnan rekisteröinti ja kirjaudu Azure AD-käyttäjien ja paikallisten resurssit.  


## <a name="deployment-considerations-for-azure-ad-join"></a>Käyttöönottoon liittyviä huomioita Azure AD liittyminen

### <a name="enable-your-users-to-join-a-company-owned-device-directly-to-azure-ad"></a>Ottaa käyttöön liitos yrityksen omistamien laite suoraan Azure AD-käyttäjille


Yrityksille tarjota vain pilvipalveluita tilit kumppaniyritykset ja organisaatioissa. Yhteistyökumppanien voit helposti käyttää yrityksen sovellukset ja kertakirjautumisen resursseilla. Tässä skenaariossa on ensisijaisesti pilvipalveluun, kuten Office 365- tai SaaS sovellukset, jotka käyttävät todennusta varten Azure AD-resursseja käyttävien käyttäjien käytettävissä.

### <a name="prerequisites"></a>Edellytykset
**Yrityksen tasolla (järjestelmänvalvoja)**

*   Azure tilauksen Azure Active Directory-hakemistosta  

**Käyttäjän tasolla**

*   Windows 10: ssä (Professional- ja Enterprise-versio)

### <a name="administrator-tasks"></a>Järjestelmänvalvojan tehtävät
* [Laitteen rekisteröinnin määrittäminen](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Käyttäjän tehtävät
* [Määrittää uuden Windows 10-laitteen kanssa Azure AD asennuksen aikana](active-directory-azureadjoin-user-frx.md)
* [Määritä asetukset-valikossa Azure AD kanssa Windows 10-laitteeseen](active-directory-azureadjoin-user-upgrade.md)
* [Liittäminen organisaation omien Windows 10-laitteeseen](active-directory-azureadjoin-personal-device.md)



## <a name="enable-byod-in-your-organization-for-windows-10"></a>Ota käyttöön BYOD organisaation Windows 10: lle
Voit määrittää käyttäjiä ja työntekijöiden henkilökohtaisten Windows-laitteiden (BYOD), voit käyttää yrityksen sovellusten ja-resurssit. Käyttäjien lisätä omat Windows-laitteessa access resurssien suojatun ja yhteensopiva tavalla niiden Azure AD-tilit (työpaikan tai oppilaitoksen tilit).

### <a name="prerequisites"></a>Edellytykset
**Yrityksen tasolla (järjestelmänvalvoja)**

*   Azure AD-tilaus

**Käyttäjän tasolla**

*   Windows 10: ssä (Professional- ja Enterprise-versio)


### <a name="administrator-tasks"></a>Järjestelmänvalvojan tehtävät

* [Laitteen rekisteröinnin määrittäminen](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Käyttäjän tehtävät
* [Liittäminen organisaation omien Windows 10-laitteeseen](active-directory-azureadjoin-personal-device.md)


## <a name="additional-information"></a>Lisätietoja
* [Windows 10: n yrityksen: tapoja käyttää laitteet](active-directory-azureadjoin-windows10-devices-overview.md)
* [Cloud ominaisuuksia Windows 10-laitteisiin kautta Azure Active Directory-liittyä laajentaminen](active-directory-azureadjoin-user-upgrade.md)
* [Käyttäjätietojen ilman salasanan vaihtamiseen Microsoft Passport todennustapa](active-directory-azureadjoin-passport.md)
* [Lisätietoja käyttötapoja Azure AD liittyminen](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Toimialueen liittyneet laitteiden yhdistäminen Azure AD for Windows 10: ssä kokemukset](active-directory-azureadjoin-devices-group-policy.md)
* [Määritä Azure AD liittyminen](active-directory-azureadjoin-setup.md)
