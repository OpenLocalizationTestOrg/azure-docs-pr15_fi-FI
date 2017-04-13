<properties
    pageTitle="Hallinnasta Federation varmenteet Azure AD | Microsoft Azure"
    description="Katso, miten voit mukauttaa päättymispäivän federation varmenteiden ja uusiminen sertifikaatteja, joita pian päättyy."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="managing-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>Varmenteiden, liitetyt kertakirjautumisen Azure Active Directoryn hallinta

Tässä artikkelissa kerrotaan, että Azure Active Directory Luo vahvistamiseksi liitetyt kertakirjautuminen (SSO) SaaS sovellusten varmenteet liittyviin kysymyksiin.

Tämä artikkeli koskee vain sovellukset, jotka on määritetty käyttämään **Azure AD kertakirjautumisen**, kuten alla olevassa esimerkissä:

![Azure AD-Single Sign-On](./media/active-directory-sso-certs/fed-sso.PNG)

##<a name="how-to-customize-the-expiration-date-for-your-federation-certificate"></a>Vanhentumispäivä mukauttamisesta Federation varmenne

Oletusarvon mukaan varmenteet on määritetty vanhenevat kahden vuoden aikana. Voit valita eri vanhentumispäivä varmenteelle noudattamalla seuraavia ohjeita. Sisältää näyttökuvat käyttää Salesforce Esimerkki vuoksi, mutta liitetyt SaaS-sovelluksen käyttää näitä ohjeita.

1. Azure Active Directory-Pika-aloitus-sivulla sovellus, valitse **Määritä kertakirjautumisen**.

    ![Avaa ohjattu määritystoiminto SSO.](./media/active-directory-sso-certs/config-sso.png)

2. Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

3. Kirjoita sovelluksesi **Sign-On URL** - ja **Määritä varmennetta käytetään liitetyt kertakirjautumisen**kohdassa oleva valintaruutu. Valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On](./media/active-directory-sso-certs/new-app-config-sso.PNG)

4. Valitse seuraavalla sivulla **Luo uutta varmennetta**ja valitse, kuinka kauan haluat varmenne on voimassa. Valitse sitten **Seuraava**.

    ![Luo uutta varmennetta.](./media/active-directory-sso-certs/new-app-config-cert.PNG)

5. Valitse **Lataa**. Lisätietoja Lataa sertifikaatti tietyn SaaS-sovellukseen, valitse **Näytä määritysohjeet**.

    ![Lataa ja valitse Lataa sertifikaatti](./media/active-directory-sso-certs/new-app-config-app.PNG)

6. Varmennetta ei ole käytettävissä, kunnes valintaikkunan alareunassa vahvistus-valintaruutu ja paina sitten Lähetä.

##<a name="how-to-renew-a-certificate-that-will-soon-expire"></a>Uusiminen varmennetta, joka päättyy pian

Alla olevassa uusiminen ohjeita olisi Ihannetapauksessa johtaa ei ole merkittäviä käyttökatkot käyttäjiä. Käyttää osan toiminto Salesforce-esimerkki, mutta näin näyttökuvat käyttää liitetyt SaaS sovelluksen.

1. Azure Active Directory-Pika-aloitus-sivulla sovellus, valitse **Määritä kertakirjautumisen**.

    ![Avaa ohjattu määritystoiminto SSO](./media/active-directory-sso-certs/renew-sso-button.PNG)

2. Valintaikkunan ensimmäisellä sivulla **Azure AD kertakirjautumisen** pitäisi olla jo valittuna, joten valitse **Seuraava**.

3. Toisella sivulla kohdassa **Määritä liitetyt kertakirjautumisen käytettävä varmenne**valintaruutu. Valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On](./media/active-directory-sso-certs/renew-config-sso.PNG)

4. Valitse seuraavalla sivulla **Luo uusi varmenne**ja valitse, kuinka kauan haluat uusi varmenne on voimassa. Valitse sitten **Seuraava**.

    ![Luo uutta varmennetta.](./media/active-directory-sso-certs/new-app-config-cert.PNG)

5. Valitse **Lataa**. Voit onnistuneesti rewnew varmennetta, sinun on suoritettava seuraavat kaksi vaihetta:

    - Lataa SaaS app Sign määritysten näytön uutta varmennetta. Lisätietoja toimi samoin kunkin SaaS-sovelluksen, valitse **Näytä määritysohjeet**.

    - Azure AD-käyttöön uutta varmennetta valintaikkunan alareunassa vahvistus-valintaruutu ja valitse sitten **Seuraava** Lähetä.

    > [AZURE.IMPORTANT] Kertakirjautumisen sovellukseen eivät ole käytettävissä, näiden vaiheiden joko yhdestä hetken on valmis, mutta se otetaan käyttöön uudelleen, kun toinen vaihe on valmis. Tämän vuoksi pienentämiseksi käyttökatkot Ota valmisteleminen suoritettava molemmat vaiheet sisällä lyhyt aikamäärä päähän toisistaan.

    ![Lataa ja valitse Lataa sertifikaatti](./media/active-directory-sso-certs/renew-config-app.PNG)

## <a name="related-articles"></a>Aiheeseen liittyviä artikkeleita

- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
- [Sovelluksen käyttöoikeuksien ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md)
- [SAML-pohjaisen kertakirjautumisen vianmääritys](active-directory-saml-debugging.md)
