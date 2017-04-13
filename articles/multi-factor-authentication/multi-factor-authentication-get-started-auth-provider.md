<properties
    pageTitle="Hae aloittaminen Azure multi-factor Auth tarjoajan | Microsoft Azure"
    description="Opettele luomaan tarjoajan Auth Azure multi-factor."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>



# <a name="getting-started-with-an-azure-multi-factor-auth-provider"></a>Azure multi-factor Auth-palvelun käytön aloittaminen
Kaksivaiheinen vahvistus on oletusarvoisesti yleinen järjestelmänvalvojille, jotka on Azure Active Directory- ja Office 365-käyttäjille. Jos haluat hyödyntää [Lisäominaisuudet](multi-factor-authentication-whats-next.md) olisi ostat sitten täysi versio Azure multi-factor Authentication (MFA).

> [AZURE.NOTE]  Azure multi-factor Auth tarjoajan avulla voit hyödyntää Azure MFA täyden version ominaisuudet. Käyttäjien on joka **ei ole käyttöoikeuksia Azure MFA, Azure AD Premium tai EMS kautta**.  Azure MFA, Azure AD Premium ja EMS sisältävät täydellisen version Azure MFA oletusarvoisesti.  Jos sinulla on käyttöoikeudet, valitse sinun ei tarvitse tarjoajan Auth Azure multi-factor.

Azure multi-factor Auth tarjoajan tarvitaan ladata SDK.

> [AZURE.IMPORTANT]  Voit ladata SDK-luominen tarjoajan Auth Azure multi-factor, vaikka sinulla olisi Azure MFA, AAD Premium tai EMS käyttöoikeudet.  Jos luot Azure multi-factor Auth tarjoajan tähän tarkoitukseen ja on jo käyttöoikeudet, muista Luo palvelu **Käyttöön käyttäjään** mallia. Linkitä palveluntarjoajan kansioon, joka sisältää Azure MFA, Azure AD Premium tai EMS-käyttöoikeudet.  Näin varmistat, että sinulla on vain laskuttaa Jos sinulla on useita eri käyttäjää käyttämällä SDK kuin omistat käyttöoikeuksien määrä.


## <a name="to-create-a-multi-factor-auth-provider"></a>Voit luoda multi-factor Auth palveluntarjoaja

Seuraavien vaiheiden avulla voit luoda tarjoajan Auth Azure multi-factor.

1. Kirjaudu [Azure perinteinen portaaliin](https://manage.windowsazure.com) järjestelmänvalvojana.
2. Valitse vasemmassa reunassa **Active Directorysta**.
3. Valitse Active Directory-sivun yläreunassa, **Multi-Factor käyttöoikeustarkistuspalvelun**.
![MFA-palvelun luominen](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)
4. Ja valitse **Uusi**.
![MFA-palvelun luominen](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)
5. Valitse sovelluksen palvelut-Valitse **Multi-Factor Auth tarjoaja**
![MFA-palvelun luominen](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)
6. Valitse **nopea luominen**.
![MFA-palvelun luominen](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)
5. Täytä seuraavat kentät ja valitse **Luo**.
    1. **Nimi** – multi-factor Auth tarjoajan nimi.
    2. **Käyttö mallia** , haluatko yksittäiset käyttäjät tai maksa vahvistus kohden. Valitse jokin seuraavista vaihtoehdoista:
        - Kohti todennus – ostamisen malli, joka kulujen todennus kohden. Käytetään yleensä tilanteita, joissa käyttää Azure Monimenetelmäisen todentamisen kuluttaja osoittava-sovelluksessa.
        - Käytössä käyttäjäkohtainen – ostamisen malli, joka kulujen kohti käytössä käyttäjä. Käytetään yleensä työntekijöiden käyttöoikeuksia sovelluksista, kuten Office 365: ssä. Valitse tämä vaihtoehto, jos sinulla on kaikki käyttäjät, joilla on jo käyttöoikeus Azure MFA-Todentamista varten.
    2. **Hakemiston** – Azure Active Directory-alihallintaympäristöä, johon multi-factor Authentication-palvelu on liitetty. Ota huomioon seuraavat:
        - Sinun ei tarvitse Azure AD-directory multi-factor Auth palveluntarjoaja luomiseen. Jätä ruutu tyhjäksi, jos aiot vain Azure multi-factor Authentication Serverin tai SDK.
        - Multi-factor Auth-palvelu on liitettävä Azure AD-kansio, jotta voit hyödyntää lisäominaisuudet.
        - Azure AD Connect, AAD Sync tai DirSync ovat vain tarpeen, jos synkronoitu paikallisen Active Directory-ympäristön Azure Active Directory-hakemistosta.  Jos käytössäsi on vain Azure AD-kansio, joka ei ole synkronoitu, tämä ei ole pakollinen.
![MFA-palvelun luominen](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)
5. Kun napsautat luominen, Multi-factor Authentication-palvelu on luotu ja näkyviin tulee viesti, jossa sanotaan: **multi-factor Authentication-palvelu on luotu**. Valitse **Ok**.
![MFA-palvelun luominen](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)
