<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi NetSuite | Microsoft Azure"
    description="Opettele käyttämään NetSuite Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="05/16/2016"
    ms.author="asmalser-msft"/>

#<a name="tutorial-how-to-integrate-netsuite-with-azure-active-directory"></a>Opetusohjelma: Miten voit integroida NetSuite Azure Active Directory-hakemistosta

Tässä opetusohjelmassa kerrotaan, kuinka NetSuite ympäristön muodostaa Azure Active Directory (Azure AD). Opit määrittäminen Kirjaudu NetSuite-, miten voit ottaa käyttöön automaattisen käyttäjän valmistelu ja käyttäjillä on pääsy NetSuite määrittäminen. 

##<a name="prerequisites"></a>Edellytykset

1. Azure Active Directory-palvelun [Azure perinteinen portal](https://manage.windowsazure.com)-käyttöön on kelvollinen Azure tilaus.

2. Sinulla on järjestelmänvalvojan käyttöoikeudet [NetSuite](http://www.netsuite.com/portal/home.shtml) -tilaukseen. Ilmainen kokeiluversio tili saattaa käytettäväksi joko-palvelussa.

##<a name="step-1-add-netsuite-to-your-directory"></a>Vaihe 1: Lisää NetSuite-kansioon

1. Valitse **Active Directory** [Azure perinteinen portal](https://manage.windowsazure.com), valitse vasemmassa siirtymisruudussa.

    ![Valitse Active Directory vasemmanpuoleisessa siirtymisruudussa.][0]

2. **Hakemisto** -luettelosta, jotka haluat NetSuite, jos haluat lisätä hakemiston.

3. Valitse **sovellusten** yläreunan-valikossa.

    ![Valitse sovellukset.][1]

4. Valitse sivun alareunassa **Lisää** .

    ![Valitse Lisää uusi sovellus.][2]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Valitse Lisää sovellus-valikoimasta.][3]

6. Kirjoita **hakuruutuun** **NetSuite**. Valitse Valitse **NetSuite** tulokset ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Lisää NetSuite.][4]

7. Pika-aloitus-sivun pitäisi tulla näkyviin NetSuite varten:

    ![Azure AD NetSuite's Pika-aloitus sivulle][5]

##<a name="step-2-enable-single-sign-on"></a>Vaihe 2: Ottaminen käyttöön

1. Napsauta Azure AD-NetSuite, pika-aloitus-sivun **Määritä single Sign** -painiketta.

    ![Määritä yhden Sign-painike][6]

2. Valintaikkuna avautuu, ja näkyviin viesti, jossa kysytään, "Siitä, miten haluat käyttäjät kirjautumaan NetSuite?" Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Valitse Azure AD-Single Sign-On][7]

    > [AZURE.NOTE] Lisätietoja eri yksittäisen Sign-asetuksista, [napsauttamalla tätä](../active-directory-appssoaccess-whatis/#how-does-single-sign-on-with-azure-active-directory-work)

3. Kirjoita **Määrittää sovelluksen asetukset** -sivun **Vastaa URL-osoite** -kenttään NetSuite vuokraajan URL-Osoitteesi jollakin seuraavista muodoista:
    - `https://<tenant-name>.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na1.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na2.netsuite.com/saml2/acs`
    - `https://<tenant-name>.sandbox.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`

    ![Kirjoita vuokraajan URL-osoite][8]

4. Valitse **Määritä kertakirjautumisen osoitteessa NetSuite** -sivulla valitsemalla **Lataa metatiedot**ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Lataa metatiedot.][9]

5. Avaa uudessa välilehdessä selaimessa ja kirjaudu sisään järjestelmänvalvojana Netsuite yrityksesi sivuston.

6. Valitse sivun yläosassa työkalurivillä **asetukset**ja valitse sitten **Asennuksen hallinta**.

    ![Siirry asennuksen hallinta][10]

7. Valitse **Integration** **Määritystehtäviä** -luettelosta.

    ![Siirry-integrointiin][11]

8. Valitse **Käyttöoikeuksien hallinta** -osassa **SAML Single Sign-on**.

    ![Siirry SAML kertakirjautuminen][12]

9. **SAML-määritys** -sivulle toimimalla seuraavasti:

    - Azure Active Directoryn **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se NetSuite **Tunnistetietojen toimittaja-kirjautumissivu** -kenttään.

        ![SAML-asetukset-sivu.][13]

    - Valitse NetSuite, **Ensisijainen todentamismenetelmän**.

    - Valitse **Lataa IDP metatietojen tiedosto**, jossa **SAMLV2 tunnistetietojen toimittaja metatiedot**kentälle. Valitse vaiheessa #4 lataamasi metatiedot-tiedoston lataaminen **Selaa** .

        ![Lataa metatiedot][16]

    - Valitse **Lähetä**.

9. Valitse Azure AD-varmenne, jota olet ladannut NetSuite käyttöön Sign määritysten vahvistus-valintaruutu. Valitse sitten **Seuraava**.

    ![Valitse vahvistus-valintaruutu][14]

10. Valintaikkunan viimeisellä sivulla Kirjoita sähköpostiosoite, jos haluat saada sähköposti-ilmoitukset virheiden ja varoitusten liittyvät yksittäisen Sign määritysten ylläpito. 

    ![Kirjoita sähköpostiosoitteesi.][15]

11. Valitse **Valmis** , sulje valintaikkuna. Valitse seuraavaksi **määritteet** sivun yläreunassa.

    ![Valitse määritteet.][17]

12. Valitse **Lisää käyttäjä-määrite**.

    ![Valitse Lisää käyttäjä-määrite.][18]

13. Kirjoita **Määritteen nimi** -kenttään `account`. **Määritteen arvo** kentälle Kirjoita käyttämällä NetSuite tili. Ohjeita siitä, miten voit etsiä tilin tunnus on kuvattu seuraavassa:

    ![Lisää käyttämällä NetSuite tili.][19]

    - Valitse NetSuite, **määritys** yläsiirtymispalkissa-valikosta.
    - Valitse vasemmanpuoleisessa siirtymisruudussa **Asetukset** -kohdassa, valitse **integrointi** -osassa ja valitse **Web Services**asetusten mukaisesti.
    - Kopioi NetSuite Tilitunnus ja liitä se Azure AD- **Määrite-arvo** -kenttään.

        ![Tili-tunnuksen hankkiminen][20]

14. Valitse Azure AD- **valmiina** Viimeistele lisääminen SAML-määrite. Valitse **Ota muutokset käyttöön** ala-valikosta.

    ![Tallenna muutokset.][21]

15. Ennen kuin käyttäjät voivat suorittaa kertakirjautumisen NetSuite kyselyjä, ne on ensin määritettävä NetSuite on tarvittavat käyttöoikeudet. Noudattamalla seuraavia ohjeita voit määrittää nämä oikeudet.

    - Valitse yläsiirtymispalkista-valikossa **asetukset**ja valitse sitten **Asennuksen hallinta**.

        ![Siirry asennuksen hallinta][10]

    - Valitse vasemmanpuoleisessa siirtymisruudussa **Käyttäjät/roolit**ja valitse **Roolien hallinta**.

        ![Siirry roolien hallinta][22]

    - Valitse **uusi rooli**.

    - Kirjoita **nimi** voit uusi rooli ja valitse **Yksittäinen Sign-On vain** -valintaruutu.

        ![Nimeä uusi rooli.][23]

    - Valitse **Tallenna**.

    - Valitse ylä-valikossa Valitse **käyttöoikeudet**. Valitse **asetukset**.

        ![Siirry käyttöoikeudet][24]

    - Valitse **Määritä määrittäminen SAM kertakirjautumisen**ja valitse sitten **Lisää**.

    - Valitse **Tallenna**.

    - Valitse yläsiirtymispalkista-valikossa **asetukset**ja valitse sitten **Asennuksen hallinta**.

        ![Siirry asennuksen hallinta][10]

    - Vasemmasta valikosta **Käyttäjät/roolit**ja valitse sitten **Käyttäjien hallinta**.

        ![Siirry käyttäjien hallinta][25]

    - Valitse testikäyttäjän. Valitse **Muokkaa**.

        ![Siirry käyttäjien hallinta][26]

    - Valitse roolit-valintaikkunassa Valitse rooli, että olet luonut ja sitten **Lisää**.

        ![Siirry käyttäjien hallinta][27]

    - Valitse **Tallenna**.

19. Testaa kokoonpanosi, katso jäljempänä olevaan kohtaan [NetSuite Määritä käyttäjät](#step-4-assign-users-to-netsuite).

##<a name="step-3-enable-automated-user-provisioning"></a>Vaihe 3: Ota käyttöön automaattinen käyttäjän valmistelu

> [AZURE.NOTE] Oletusarvon mukaan valmistellun käyttäjät lisätään pääkansion tytäryhtiön NetSuite ympäristön.

1. Azure Active Directory-Pika-aloitus-sivun NetSuite, valitse **Määritä käyttäjän valmistelu**.

    ![Määrittää käyttäjän valmistelu][28]

2. -Valintaikkunassa, joka avautuu, kirjoita järjestelmänvalvojan tunnistetietoja NetSuite ja valitse sitten **Seuraava**.

    ![Kirjoita NetSuite järjestelmänvalvojan tunnistetietoja.][29]

3. Vahvistussivulla Kirjoita sähköpostiosoitteesi paikkaan, saat ilmoituksen valmistelun virheet.

    ![Vahvista.][30]

4. Valitse **Valmis** , sulje valintaikkuna.

##<a name="step-4-assign-users-to-netsuite"></a>Vaihe 4: Käyttäjien määrittäminen NetSuite

1. Voit esikatsella kokoonpanosi-Aloita testi uuden tilin luominen hakemistossa.

2. Valitse NetSuite Pika-aloitus-sivulla **Määritä käyttäjät** -painiketta.

    ![Valitse Määritä käyttäjät][31]

3. Valitse Testaa käyttäjä ja valitse näytön alareunassa **Liitä** -painiketta:

 - Jos et ole ottaminen automaattinen käyttäjän valmistelu, sitten näyttöön tulee seuraava ohjelma pyytää vahvistamaan:

        ![Confirm the assignment.][32]

 - Jos käytössä on automaattinen käyttäjän valmistelu, sinun tulee kehote määrittää tyypin, roolin käyttäjän oltava NetSuite. Äskettäin valmistellun käyttäjien pitäisi näkyä NetSuite ympäristön muutaman minuutin kuluttua.

4. Voit testata yksittäisen Sign-asetuksia, Avaa Access-paneeli osoitteessa [https://myapps.microsoft.com](https://myapps.microsoft.com/), kirjaudu sisään Testaa tilin ja valitse **NetSuite**.

##<a name="related-articles"></a>Aiheeseen liittyviä artikkeleita

- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
- [Luettelo siitä, miten voit integroida SaaS sovellusten opetusohjelmat](active-directory-saas-tutorial-list.md)

[0]: ./media/active-directory-saas-netsuite-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-netsuite-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-netsuite-tutorial/add-app.png
[3]: ./media/active-directory-saas-netsuite-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-netsuite-tutorial/add-netsuite.png
[5]: ./media/active-directory-saas-netsuite-tutorial/quick-start-netsuite.png
[6]: ./media/active-directory-saas-netsuite-tutorial/config-sso.png
[7]: ./media/active-directory-saas-netsuite-tutorial/sso-netsuite.png
[8]: ./media/active-directory-saas-netsuite-tutorial/sso-config-netsuite.png
[9]: ./media/active-directory-saas-netsuite-tutorial/config-sso-netsuite.png
[10]: ./media/active-directory-saas-netsuite-tutorial/ns-setup.png
[11]: ./media/active-directory-saas-netsuite-tutorial/ns-integration.png
[12]: ./media/active-directory-saas-netsuite-tutorial/ns-saml.png
[13]: ./media/active-directory-saas-netsuite-tutorial/ns-saml-setup.png
[14]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-confirm.png
[15]: ./media/active-directory-saas-netsuite-tutorial/sso-email.png
[16]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-setup.png
[17]: ./media/active-directory-saas-netsuite-tutorial/ns-attributes.png
[18]: ./media/active-directory-saas-netsuite-tutorial/ns-add-attribute.png
[19]: ./media/active-directory-saas-netsuite-tutorial/ns-add-account.png
[20]: ./media/active-directory-saas-netsuite-tutorial/ns-account-id.png
[21]: ./media/active-directory-saas-netsuite-tutorial/ns-save-saml.png
[22]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-roles.png
[23]: ./media/active-directory-saas-netsuite-tutorial/ns-new-role.png
[24]: ./media/active-directory-saas-netsuite-tutorial/ns-sso.png
[25]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-users.png
[26]: ./media/active-directory-saas-netsuite-tutorial/ns-edit-user.png
[27]: ./media/active-directory-saas-netsuite-tutorial/ns-add-role.png
[28]: ./media/active-directory-saas-netsuite-tutorial/netsuite-provisioning.png
[29]: ./media/active-directory-saas-netsuite-tutorial/ns-creds.png
[30]: ./media/active-directory-saas-netsuite-tutorial/ns-confirm.png
[31]: ./media/active-directory-saas-netsuite-tutorial/assign-users.png
[32]: ./media/active-directory-saas-netsuite-tutorial/assign-confirm.png
