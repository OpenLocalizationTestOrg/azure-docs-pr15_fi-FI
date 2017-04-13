<properties
    pageTitle="Kirjaudu sisään ilmenee Azure AD-tunnistetiedot | Microsoft Azure"
    description="Tässä artikkelissa on yleiskatsaus käyttäjäkokemuksen, kun tunnistetiedot on, joiden lievity tai remediated käyttäjän tai käytäntö edellyttää monimenetelmäisen todentamisen."
    services="active-directory"
    keywords="Azure active Directoryn tunnistetietojen suojaus-cloud app etsiminen-sovellukset, suojaus, riski, riskin taso, heikkous, suojauskäytäntö hallinta"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="markvi"/>

# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a>Kirjaudu sisään kokemukset Azure AD-tunnistetiedot

Azure Active Directory tunnistetietojen suojauksen voit tehdä seuraavia toimia:

- Voit rekisteröidä monimenetelmäisen todentamisen pakolliseksi

- riskialttiit Kirjaudu laajennukset ja vaarantuneen käyttäjät

Järjestelmän vastausta ongelmaan ei ole vaikutusta käyttäjän kirjautuminen, koska vain suoraan kirjautua sisään antamalla käyttäjätunnusta ja salasanaa ei voi enää. Hae käyttäjän turvallisesti tarvitaan lisätoimia takaisin business.

Tässä artikkelissa tutustutaan yleiskatsaus käyttäjän kirjautuminen kaikissa tapauksissa, joita voi ilmetä.

**Monimenetelmäisen todentamisen**

- Monimenetelmäisen todentamisen rekisteröinti



**Kirjaudu sisään vastuullasi**

- Riskialttiit kirjautumisen palauttaminen

- Riskialttiit-kirjautuminen estetty

- Monimenetelmäisen todentamisen rekisteröinti riskialtis kirjautumisvirheiden aikana
 

**Käyttäjän vastuullasi**

- Vaarantuneen tilin palautus

- Vaarantuneen tili on estetty




## <a name="multi-factor-authentication-registration"></a>Monimenetelmäisen todentamisen rekisteröinti

Paras käyttäjäkokemuksen sekä vaarantuneen tilin palautus-työnkulku ja riskialtis Kirjaudu sisään-työnkulku on, kun käyttäjä itse palauttaa. Jos käyttäjiä on rekisteröity monimenetelmäisen todentamisen, heillä on yhdistetty niiden tili, jonka avulla voidaan siirtää haasteisiin puhelinnumero. Ohje ei ole pöytäpuhelimella tai järjestelmänvalvojan osallistuminen tarvitaan tilin haavoittuvan palauttaminen. Näin ollen on erittäin suositeltavaa Kehota käyttäjiä monimenetelmäisen todentamisen rekisteröity. 

Järjestelmänvalvojat voivat:

- määrittää käytännön, joka edellyttää, että käyttäjät voivat määrittää niiden tilit lisäsuojauksen vahvistusta varten. 
- Salli ohittaminen monimenetelmäisen todentamisen rekisteröinti enintään 30 päivän siltä varalta, että he haluavat käyttäjille on sallittu vapaa käyttö ennen rekisteröitymistä.

**Monimenetelmäisen todentamisen rekisteröinti on kolme vaihetta:**

1. Ensimmäinen vaihe käyttäjä saa ilmoituksen vaatimus tilin käyttäjäksi monimenetelmäisen todentamisen määrittäminen. 

    ![Korjaus] (./media/active-directory-identityprotection-flows/140.png "Korjaus")


2. Monimenetelmäisen todentamisen määrittäminen, sinun täytyy antaa järjestelmän tietää, miten haluat voi ottaa yhteyttä.

    ![Korjaus] (./media/active-directory-identityprotection-flows/141.png "Korjaus")
 
3. Järjestelmä lähettää hankala, ja sinun on vastattava.

    ![Korjaus] (./media/active-directory-identityprotection-flows/142.png "Korjaus")

 



## <a name="risky-sign-in-recovery"></a>Riskialttiit kirjautumisen palauttaminen

Kun järjestelmänvalvoja on määrittänyt käytännön kirjautumisen riskejä, tarvittavien käyttäjien ilmoitetaan, kun sisään. 

**Riskialttiit kirjautumisen työnkulku on kaksi vaihetta:** 

1. Käyttäjä saa ilmoituksen, että jotakin epätavallisia löytyi tietoja niiden kirjautumisnimi, kuten uuteen sijaintiin, laitteen tai sovelluksen kirjautuminen. 

    ![Korjaus] (./media/active-directory-identityprotection-flows/120.png "Korjaus")

2. Käyttäjän tarvitaan todista lisätoimenpiteisiin jäsenyyden mukaan suojauksen hankala ratkaisemiseen. Jos käyttäjä on rekisteröity monimenetelmäisen todentamisen ne on ohjataan uudelleen suojauskoodi niiden puhelinnumeroon. Koska kyseessä vain riskialtis Kirjaudu sisään ja vaarantuneen tilin, käyttäjän ei tarvitse tässä työnkulussa salasanan vaihtaminen. 

    ![Korjaus] (./media/active-directory-identityprotection-flows/121.png "Korjaus")



 
## <a name="risky-sign-in-blocked"></a>Riskialttiit-kirjautuminen estetty
Järjestelmänvalvojat voit myös valita kirjautumisen riskin käytäntö määritetään yhteydessä Kirjautumisvirheitä aiheuttavien roskapostisuodattimeen määritetyn riskin käyttäjien estäminen. Pääset esto loppukäyttäjät Ota yhteyttä järjestelmänvalvojaan tai -tukipalveluun tai voit yrittää kirjautua sisään tuttuja sijainti tai laitteesta. Itse palauttaminen mukaan ratkaiseminen monimenetelmäisen todentamisen vaihtoehto ei ole tässä tapauksessa.

![Korjaus] (./media/active-directory-identityprotection-flows/200.png "Korjaus")




## <a name="compromised-account-recovery"></a>Vaarantuneen tilin palautus

Kun käyttäjän riskin suojauskäytäntö on määritetty, käyttäjät, jotka täyttävät käyttäjän julkisuuteen käytännön tasoa (ja näin ollen oletetaan käsiin) on käydä läpi käyttäjän haavoittuvan palautus-työnkulku, ennen kuin he voivat kirjautua sisään. 

**Käyttäjän haavoittuvan palautus-työnkulku on kolme vaihetta:**

1. Käyttäjä saa ilmoituksen, että niiden tilin suojaus on vastuullasi vuoksi epäilyttävistä aktiviteetti tai vuotamaan tunnistetiedot.

    ![Korjaus] (./media/active-directory-identityprotection-flows/101.png "Korjaus")

2.  Käyttäjän tarvitaan todista lisätoimenpiteisiin jäsenyyden mukaan suojauksen hankala ratkaisemiseen. Jos käyttäjä on rekisteröity monimenetelmäisen todentamisen ne itse palauttaminen tartunnan. Tiedot on ohjataan uudelleen suojauskoodi niiden puhelinnumeroon. 

    ![Korjaus] (./media/active-directory-identityprotection-flows/110.png "Korjaus")


3.  Lopuksi käyttäjän pakko vaihtaa oman salasanansa, koska joku muu on ollut niiden tilin käyttöoikeus. Tämä ongelma näyttökuvat ovat jäljempänä.
 
    ![Korjaus] (./media/active-directory-identityprotection-flows/111.png "Korjaus")



## <a name="compromised-account-blocked"></a>Vaarantuneen tili on estetty 

Saat käyttäjä, jolla on estänyt käyttäjän riskin suojauskäytäntö esto-käyttäjä on, ota yhteyttä järjestelmänvalvojaan tai tukipalvelu. Itse palauttaminen mukaan ratkaiseminen monimenetelmäisen todentamisen vaihtoehto ei ole tässä tapauksessa.


![Korjaus] (./media/active-directory-identityprotection-flows/104.png "Korjaus")



 
## <a name="reset-password"></a>Salasanan vaihtaminen

Jos vaarantuneen käyttäjät on estetty kirjautumisessa, järjestelmänvalvoja voi luoda niiden tilapäinen salasana. Käyttäjien on vaihtaa oman salasanansa aikana seuraavan kirjautumisnimi.

![Korjaus] (./media/active-directory-identityprotection-flows/160.png "Korjaus")


 




 

## <a name="see-also"></a>Katso myös

- [Azure Active Directory-tunnistetietojen suojaus](active-directory-identityprotection.md) 