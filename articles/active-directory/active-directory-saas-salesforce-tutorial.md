<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Salesforce | Microsoft Azure"
    description="Opettele käyttämään Salesforce Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!"
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

#<a name="tutorial-how-to-integrate-salesforce-with-azure-active-directory"></a>Opetusohjelma: Miten voit integroida Salesforce Azure Active Directory-hakemistosta

Tässä opetusohjelmassa kerrotaan, kuinka yhteyden Salesforce-ympäristösi Azure Active Directory. Opit määrittäminen Kirjaudu Salesforce-, automaattinen käyttäjän valmistelu ottaminen käyttöön ja määrittäminen käyttäjillä on pääsy Salesforce.

##<a name="prerequisites"></a>Edellytykset

1. Azure Active Directory-palvelun [Azure perinteinen portal](https://manage.windowsazure.com)-käyttöön on kelvollinen Azure tilaus.

2. Sinulla on kelvollinen vuokraajan [Salesforce.comissa](https://www.salesforce.com/).

> [AZURE.IMPORTANT] Jos käytät **Salesforce.com kokeilutili** , valitse et voi määrittää Automaattinen käyttäjän valmistelu. Kokeiluversion tilit ei ole tarvittavia käyttöoikeuksia API-vasta, kun ne on ostettu.
> 
> Saat tämän rajoituksen avulla [ilmaista sovelluskehittäjän tilin](https://developer.salesforce.com/signup) suorittamiseen tässä opetusohjelmassa.

Jos käytät Salesforce hallitussa ympäristössä, katso [Salesforce eristetyn integrointi opetusohjelma](https://go.microsoft.com/fwLink/?LinkID=521879).

##<a name="video-tutorials"></a>Opetusvideot

Saattaa Katso tämä opetusohjelma, käyttäen alla olevia videoita.

**Videon opetusohjelma osa yksi: miten voit ottaa käyttöön kertakirjautumisen**

> [AZURE.VIDEO integrating-salesforce-with-azure-ad-how-to-enable-single-sign-on]

**Videon opetusohjelma osa kaksi: Miten voit automatisoida käyttäjän valmistelu**

> [AZURE.VIDEO integrating-salesforce-with-azure-ad-how-to-automate-user-provisioning]

##<a name="step-1-add-salesforce-to-your-directory"></a>Vaihe 1: Lisää Salesforce-kansioon

1. Valitse **Active Directory** [Azure perinteinen portal](https://manage.windowsazure.com), valitse vasemmassa siirtymisruudussa.

    ![Valitse Active Directory vasemmanpuoleisessa siirtymisruudussa.][0]

2. Valitse **Hakemisto** -luettelosta, jotka haluat Salesforce, jos haluat lisätä hakemiston.

3. Valitse **sovellusten** yläreunan-valikossa.

    ![Valitse sovellukset.][1]

4. Valitse sivun alareunassa **Lisää** .

    ![Valitse Lisää uusi sovellus.][2]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Valitse Lisää sovellus-valikoimasta.][3]

6. Kirjoita **hakuruutuun** **Salesforce**. Valitse Valitse **Salesforce** tuloksista, ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Lisää Salesforce.][4]

7. Pika-aloitus-sivun pitäisi tulla näkyviin Salesforce varten:

    ![Azure AD Salesforce's Pika-aloitus sivulle][5]

##<a name="step-2-enable-single-sign-on"></a>Vaihe 2: Ottaminen käyttöön

1. Ennen kuin voit määrittää kertakirjautumisen, määrittäminen ja ottaa käyttöön mukautetun toimialueen Salesforce-ympäristössä. Katso ohjeet siitä, miten voit tehdä sen [Toimialuenimi](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_setup.htm&language=en_US).

2. Napsauta Salesforce's Pika-aloitus Azure AD-sivulla **Määritä kertakirjautumisen** -painiketta.

    ![Määritä yhden Sign-painike][6]

3. Valintaikkuna avautuu, ja näkyviin viesti, jossa kysytään, "Siitä, miten haluat käyttäjät kirjautumaan Salesforce?" Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Valitse Azure AD-Single Sign-On][7]

    > [AZURE.NOTE] Lisätietoja eri yksittäisen Sign-asetuksista, [napsauttamalla tätä](../active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)

4. **Määritä sovelluksen asetukset** -sivulla Täytä **Merkki-URL-Osoitteen** kirjoittamalla Salesforce toimialueen URL-osoite seuraavassa muodossa:
 - Enterprise-tili:`https://<domain>.my.salesforce.com`
 - Kehitystyökalut-tili:`https://<domain>-dev-ed.my.salesforce.com` 

    ![Kirjoita URL-osoitetta rekisteröityminen][8]

5. Valitse **Määritä kertakirjautumisen osoitteessa Salesforce** -sivulla valitsemalla **Lataa**ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Lataa][9]

6. Avaa uudessa välilehdessä selain ja kirjaudu sisään Salesforce-järjestelmänvalvojan tilille.

7. Kohdassa **järjestelmänvalvoja** -siirtymisruudun Laajenna liittyvän osa **Turvaominaisuudet** . Valitse **Sign-**asetukset.

    ![Valitse yksittäinen Sign-asetussivulla turvaominaisuudet][10]

8. Valitse **Sign-On asetukset** -sivun **Muokkaa** -painiketta.

    ![Valitse Muokkaa-painike][11]

    > [AZURE.NOTE] Jos et voi ottaa käyttöön kertakirjautumisen asetukset Salesforce-tilisi, joudut ehkä Salesforce's tuelta jotta on otettu käyttöön-toiminto.

9. Valitse **SAML käytössä**, ja valitse sitten **Tallenna**.

    ![Valitse SAML käytössä][12]

10. SAML yksittäisen kertakirjauspalvelun asetusten määrittämiseen valitsemalla **Uusi**.

    ![Valitse SAML käytössä][13]

11. Varmista **SAML yksittäisen Sign-asetus Muokkaa** -sivulla seuraavat määritykset:

    ![Näyttökuva määrityksistä, jotka kannattaa tehdä][14]

 - Kirjoita **nimi** -kenttään määritysten kutsumanimi. Tarjoaa arvon varten **nimi** täyttäminen automaattisesti **API nimi** -tekstiruutuun.

 - Azure AD-kopioi **Myöntäjä URL-osoite** -arvo ja liitä se Salesforce **myöntäjä** -kenttään.

 - Kirjoita **kohteen tunnus tekstiruudun**Salesforce toimialuenimen käyttämällä seuraavaa kaavaa:
     - Enterprise-tili:`https://<domain>.my.salesforce.com`
     - Kehitystyökalut-tili:`https://<domain>-dev-ed.my.salesforce.com`

 - Valitse **Selaa** tai **Valitse tiedosto** Avaa **Ladattava tiedosto** -valintaikkuna, valitse Salesforce-varmenne ja valitse sitten **Avaa** Lataa sertifikaatti.

 - **SAML tunnistetietojen tyyppi**-asetukseksi **vahvistus on käyttäjän salesforce.com käyttäjänimi**.

 - **SAML tunnistetietojen sijaintiin**Valitse **tunnistetiedot on NameIdentifier osan aihe-lause**

 - Azure AD- **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se Salesforce- **Tunnistetietojen toimittaja sisäänkirjautumisen URL-osoite** -kenttään.

 - **Palveluntarjoajan aloittanut pyytää sidonta**Valitse **HTTP-uudelleenohjaus**.

 - Valitse lopuksi sovelletaan SAML yksittäisen kertakirjauspalvelun asetusten **tallentaminen** .

12. Salesforce vasemmassa siirtymisruudussa Laajenna liittyvän osa **Toimialuehallinta** ja valitse sitten **Oma toimialue**.

    ![Valitse toimialueellani][15]

13. Siirry **Todennuksen määrittäminen** -osioon ja valitse **Muokkaa** -painiketta.

    ![Valitse Muokkaa-painike][16]

14. **Todentamispalvelu** -kohdassa Valitse kutsumanimi SAML SSO-määritys ja valitse sitten **Tallenna**.

    ![Valitse SSO-määritys][17]

    > [AZURE.NOTE] Jos useamman kuin yhden Todentamispalvelu on valittuna, valitse käyttäjä yrittää käynnistää kertakirjautumisen Salesforce-ympäristöön ne pyydetään valitsemaan Todentamispalvelu, mitä ne haluat kirjautua sisään. Jos et halua tätä, valitse kannattaa **jättää kaikki muut todennuspalvelujen ole valittuna**.

15. Valitse Azure AD-käyttöön varmenne, jota olet ladannut Salesforce Sign määritysten vahvistus-valintaruutu. Valitse sitten **Seuraava**.

    ![Valitse vahvistus-valintaruutu][18]

16. Valintaikkunan viimeisellä sivulla Kirjoita sähköpostiosoite, jos haluat saada sähköposti-ilmoitukset virheiden ja varoitusten liittyvät yksittäisen Sign määritysten ylläpito. 

    ![Kirjoita sähköpostiosoitteesi.][19]

17. Valitse **Valmis** , sulje valintaikkuna. Testaa kokoonpanosi, katso jäljempänä olevaan kohtaan [Salesforce Määritä käyttäjät](#step-4-assign-users-to-salesforce).

##<a name="step-3-enable-automated-user-provisioning"></a>Vaihe 3: Ota käyttöön automaattinen käyttäjän valmistelu

1. Napsauta Salesforce Azure AD Pika-aloitus-sivulla **Määritä käyttäjän valmistelu** -painiketta.

    ![Valitse Määritä käyttäjän valmistelu-painike][20]

2. Valitse **Määritä käyttäjän valmistelu** -valintaikkunan Kirjoita Salesforce järjestelmänvalvojan käyttäjänimi ja salasana.

    ![Kirjoita järjestelmänvalvojan käyttäjänimi tai salasana][21]

    > [AZURE.NOTE] Jos olet määrittämässä tuotantoympäristössä, parhaiden käytäntöjen mukaisesti kannattaa luoda uuden järjestelmänvalvojan tilin Salesforce-varten tämän vaiheen. Tällä tilillä on oltava **Järjestelmänvalvoja** -profiiliin määritetty Salesforce.

3. Saat Salesforce-suojauksen tunnuksen, Avaa uudessa välilehdessä ja kirjaudu saman Salesforce järjestelmänvalvojan tilille. Valitse sivun oikeassa yläkulmassa nimeäsi ja valitse sitten **Omat asetukset**.

    ![Napsauta nimeäsi ja valitse sitten Omat asetukset][22]

4. Valitse vasemmassa siirtymisruudussa valitsemalla **Omat** Laajenna liittyvän osa ja valitse sitten Valitse **Nollaa oma suojauksen salasana**.

    ![Napsauta nimeäsi ja valitse sitten Omat asetukset][23]

5. Valitse **Palauta omat suojauksen salasana** -sivulla **Palauta suojauksen salasana** -painiketta.

    ![Lue varoitukset.][24]

6. Valitse Järjestelmänvalvoja-tiliin liittyvät sähköpostin Saapuneet-kansiosta. Etsi sähköpostiviestin, jossa on uusi suojaustunnus Salesforce.comista.

7. Kopioi tunnus, siirry Azure AD-ikkunaan ja liitä se **Käyttäjän suojauksen salasana** -kenttään. Valitse sitten **Seuraava**.

    ![Liitä suojaustunnus][25]

8. Valitse Vahvista-sivulla voit vastaanottaa sähköposti-ilmoitukset valmistelu virheet toteutuessa. Valitse **Valmis** , sulje valintaikkuna.

    ![Kirjoita sähköpostiosoitteesi paikkaan, saat ilmoituksen][26]

##<a name="step-4-assign-users-to-salesforce"></a>Vaihe 4: Salesforce käyttäjien määrittäminen

1. Testaa kokoonpanosi valitsemalla Aloita testi uuden tilin luominen hakemistossa.

2. Valitse Salesforce Pika-aloitus-sivulla **Määritä käyttäjät** -painiketta.

    ![Valitse Määritä käyttäjät][27]

3. Valitse Testaa käyttäjä ja valitse näytön alareunassa **Liitä** -painiketta:

 - Jos et ole ottaminen automaattinen käyttäjän valmistelu, sitten näyttöön tulee seuraava ohjelma pyytää vahvistamaan:

        ![Confirm the assignment.][28]

 - Jos olet ottanut automaattisen käyttäjän valmistelu, sinun tulee kehote, voit määrittää, millaisia Salesforce-profiilin käyttäjän pitäisi olla. Äskettäin valmistellun käyttäjien pitäisi näkyä Salesforce-ympäristösi muutaman minuutin kuluttua.

        ![Confirm the assignment.][29]

        > [AZURE.IMPORTANT] Jos ovat valmistelu Salesforce- **Kehitystyökalut** -ympäristöön, sinun on hyvin vähän kunkin profiilin käytettävissä olevia käyttöoikeuksia. Sen vuoksi on parasta käyttäjien **Chatter vapaa** käyttäjäprofiiliin, joka on käytettävissä 4,999 tarvittavat käyttöoikeudet.

4. Voit testata yksittäisen Sign-asetuksia, Avaa Access-paneeli osoitteessa [https://myapps.microsoft.com](https://myapps.microsoft.com/), ja valitse Kirjaudu testi-tilille ja valitse **Salesforce**.

##<a name="related-articles"></a>Aiheeseen liittyviä artikkeleita

- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
- [Luettelo siitä, miten voit integroida SaaS sovellusten opetusohjelmat](active-directory-saas-tutorial-list.md)

[0]: ./media/active-directory-saas-salesforce-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-salesforce-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-salesforce-tutorial/add-app.png
[3]: ./media/active-directory-saas-salesforce-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-salesforce-tutorial/add-salesforce.png
[5]: ./media/active-directory-saas-salesforce-tutorial/salesforce-added.png
[6]: ./media/active-directory-saas-salesforce-tutorial/config-sso.png
[7]: ./media/active-directory-saas-salesforce-tutorial/select-azure-ad-sso.png
[8]: ./media/active-directory-saas-salesforce-tutorial/config-app-settings.png
[9]: ./media/active-directory-saas-salesforce-tutorial/download-certificate.png
[10]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png
[11]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png
[12]: ./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png
[13]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png
[14]: ./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png
[15]: ./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png
[16]: ./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png
[17]: ./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png
[18]: ./media/active-directory-saas-salesforce-tutorial/sso-confirm.png
[19]: ./media/active-directory-saas-salesforce-tutorial/sso-notification.png
[20]: ./media/active-directory-saas-salesforce-tutorial/config-prov.png
[21]: ./media/active-directory-saas-salesforce-tutorial/config-prov-dialog.png
[22]: ./media/active-directory-saas-salesforce-tutorial/sf-my-settings.png
[23]: ./media/active-directory-saas-salesforce-tutorial/sf-personal-reset.png
[24]: ./media/active-directory-saas-salesforce-tutorial/sf-reset-token.png
[25]: ./media/active-directory-saas-salesforce-tutorial/got-the-token.png
[26]: ./media/active-directory-saas-salesforce-tutorial/prov-confirm.png
[27]: ./media/active-directory-saas-salesforce-tutorial/assign-users.png
[28]: ./media/active-directory-saas-salesforce-tutorial/assign-confirm.png
[29]: ./media/active-directory-saas-salesforce-tutorial/assign-sf-profile.png
