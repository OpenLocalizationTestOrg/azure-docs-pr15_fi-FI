<properties
    pageTitle="Cloud resurssien Azure MFA ja AD FS"
    description="Tämä on Azure multi-factor authentication sivu, jossa kerrotaan, miten Azure MFA ja AD FS pilvipalvelussa käytön aloittaminen."
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

# <a name="securing-cloud-resources-with-azure-multi-factor-authentication-and-ad-fs"></a>Cloud resurssien Azure Monimenetelmäisen todentamisen ja AD FS suojaaminen

Jos organisaatiosi on liitetty Azure Active Directory-hakemistosta, käytä Azure Monimenetelmäisen todentamisen tai Active Directory Federation Services suojaamiseen resursseja, joita voi käyttää Azure AD mukaan. Ohjeiden avulla voit suojata Azure Active Directory-resurssien Azure Monimenetelmäisen todentamisen tai Active Directory Federation Services.

## <a name="secure-azure-ad-resources-using-ad-fs"></a>Suojattu käyttäen AD FS Azure AD-resurssit

Suojaamiseen cloud resurssin ensin käyttöön käyttäjien tili ja valitse saatavat-sääntö määritetään. Tee näin käy läpi vaiheet:

1. Tilin [Turn-on monimenetelmäisen todentamisen käyttäjille](active-directory/multi-factor-authentication-get-started-cloud.md#turn-on-multi-factor-authentication-for-users) annettujen ohjeiden avulla.
2. Käynnistä AD FS-hallintakonsoli.
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/adfs1.png)
3. Siirry **Käyttäisit osapuolen luottamussuhteet** ja napsauttamalla hiiren kakkospainikkeella käyttäisit osapuolen luota. Valitse **Muokkaa ryhmän sääntöjä...**
4. Valitse **Lisää sääntö...**
5. Avattavasta luettelosta Valitse **Lähetä käyttämällä mukautetun säännön saatavat** ja valitse **Seuraava**.
6. Kirjoita ryhmän säännön nimi.
7. Valitse mukautettu sääntö: Lisää seuraava teksti:

    ```
    => issue(Type = "http://schemas.microsoft.com/claims/authnmethodsreferences", Value = "http://schemas.microsoft.com/claims/multipleauthn");
    ```

    Vastaava vaatimus:

    ```
    <saml:Attribute AttributeName="authnmethodsreferences" AttributeNamespace="http://schemas.microsoft.com/claims">
    <saml:AttributeValue>http://schemas.microsoft.com/claims/multipleauthn</saml:AttributeValue>
    </saml:Attribute>
    ```

8. Valitse **OK** ja valitse sitten **Valmis**. Sulje AD FS-hallintakonsoli.

Käyttäjät sitten voivat suorittaa kirjautumisessa paikallisen menetelmällä (kuten älykortin).

## <a name="trusted-ips-for-federated-users"></a>Luotettu IP-osoitteet, liitetyt käyttäjät
Luotettu IP-osoitteet avulla järjestelmänvalvojat voivat ohituksen kaksivaiheinen vahvistus on IP-osoitteet tai liitetyt käyttäjät, joilla on peräisin omia intranet pyynnöt. Seuraavissa kohdissa kuvataan määrittäminen Azure multi-factor Authentication Luotetut IP-osoitteet liitetyt käyttäjät ja ohituksen kaksivaiheista vahvistusta varten, kun pyyntö on peräisin liitetyt käyttäjät intranet kuluessa. Tämä saavutetaan määrittämällä AD FS läpivienti tai suodattaa saapuvat vaatimus-malli, jonka sisällä yritysverkon ryhmän tyyppi.

Tässä esimerkissä käytetään Office 365: ssä, tutustu käyttäisit osapuolen luottaa.

### <a name="configure-the-ad-fs-claims-rules"></a>Määritä AD FS saatavat säännöt

Voit tehdä annettava ensimmäiseksi AD FS saatavat määrittämiseen. Luodaan kahden saatavat sääntöjä, toinen sisällä yritysverkon ryhmän tyypin ja muita luotua säilyttämisestä kirjautuneena käyttäjien.

1. Avaa AD FS hallinta.
2. Valitse vasemmassa reunassa **Käyttäisit osapuolen luottaa**.
3. Napsauta **Microsoft Office 365: n tunnistetietojen Platform** hiiren kakkospainikkeella ja valitse **Muokkaa ryhmän sääntöjä...** 
 ![Pilveen](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)
4. Valitse Liittoutumispalvelujen Transform säännöt **Lisää sääntö.** 
 ![Pilveen](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)
5. Valitse Muunna varaa säännön Ohjattu lisääminen Valitse **Välitä kautta tai Suodata saapuvien ryhmän** avattavasta luettelosta ja valitse **Seuraava**.
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)
6. Varaa säännön nimen vieressä olevaan ruutuun kohtaan säännön nimi. Esimerkki: InsideCorpNet.
7. Avattavasta luettelosta vieressä saapuvan postin väittää tyyppi-kohdassa **Sisällä yrityksen verkkoon**.
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)
8. Valitse **Valmis**.
9. Valitse Liittoutumispalvelujen Transform säännöt **Lisää sääntö**.
10. Valitse Muunna varaa säännön ohjattu **Lähetä saatavat käyttämällä mukautetun säännön** avattavasta luettelosta ja valitse **Seuraava**.
11. Varaa säännön nimi-ruutuun: Kirjoita *Säilyttää käyttäjät kirjautunut sisään*.
12. Kirjoita mukautettu sääntö-ruudussa:

        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip5.png)
13. Valitse **Valmis**.
14. Valitse **Käytä**.
15. Valitse **Ok**.
16. Sulje AD FS hallinta.



### <a name="configure-azure-multi-factor-authentication-trusted-ips-with-federated-users"></a>Määritä Azure multi-factor todennus Luotetut IP-osoitteet, liitetyt käyttäjien kanssa
Nyt kun saatavat ovat paikallaan, emme määrittäminen luotettujen IP-osoitteet.

1. Kirjautuminen [Azure perinteinen portal](https://manage.windowsazure.com).
2. Valitse vasemmassa reunassa **Active Directorysta**.
3. Kansio Valitse kansio, johon haluat määrittää luotettujen IP-osoitteet.
4. Valitse kansio on valittuna, **Määritä**.
5. Valitse multi-factor authentication-osassa **Hallitse Palveluasetukset**.
6. Valitse Palveluasetukset-sivulla luotettu IP-osoitteet, **ohittaa Avainhankkeiden monivaiheisen-factor todentaminen pyyntöjen liitetyt käyttäjät Omat intranet.** 
 ![Pilveen](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip6.png)
7. Valitse **Tallenna**.
8. Kun päivitykset on otettu käyttöön, valitse **Sulje**.


Joka on tämä! Tässä vaiheessa liitetyt Office 365-käyttäjille tulee olla vain käyttää MFA vaatimus on peräisin yrityksen intranetin ulkopuolella.
