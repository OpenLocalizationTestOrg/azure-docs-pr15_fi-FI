<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Citrix GoToMeeting | Microsoft Azure" 
    description="Opettele käyttämään Citrix GoToMeeting Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!." 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>

<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-citrix-gotomeeting"></a>Opetusohjelma: Citrix GoToMeeting Azure Active Directory-integrointi  
Koskee: Azure

>[AZURE.TIP]Voit antaa palautetta napsauttamalla [tätä](http://go.microsoft.com/fwlink/?LinkId=522412).

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Citrix GoToMeeting integrointi. Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Valitse Citrix GoToMeeting palvelutili

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Citrix GoToMeeting sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Määritys] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768996.png "Määritys")



##<a name="enabling-the-application-integration-for-citrix-gotomeeting"></a>Citrix GoToMeeting sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Citrix GoToMeeting sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-citrix-gotomeeting-perform-the-following-steps"></a>Citrix GoToMeeting sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-valikoimasta] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749322.png "Lisää sovellus-valikoimasta")

6.  Kirjoita **hakuruutuun** **Citrix GoToMeeting**.

    ![Citrix GoToMeeting] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701006.png "Citrix GoToMeeting")

7.  Valitse tulosruudussa **Citrix GoToMeeting**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Citrix GoToMeeting] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701012.png "Citrix GoToMeeting")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Citrix GoToMeeting käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana on base-64-koodattu varmenteen lataaminen Citrix GoToMeeting alihallintaan.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse integration **Citrix GoToMeeting** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen YKSITTÄINEN merkki käytössä** -valintaikkunan avaaminen.

    ![Ottaa käyttöön kertakirjautumisen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768997.png "Ottaa käyttöön kertakirjautumisen")

2.  Valitse **kuinka haluat käyttäjät kirjautumaan Citrix GoToMeeting** -sivulla **Microsoft Azure AD kertakirjautumisen**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768998.png "Määritä kertakirjautuminen")


3. **Määritä sovelluksen asetukset** -sivulle valitsemalla **Seuraava**. 

    ![Ottaa käyttöön kertakirjautumisen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC7689981.png "Ottaa käyttöön kertakirjautumisen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Citrix GoToMeeting** -sivulla valitsemalla **Lataa**ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768999.png "Määritä kertakirjautuminen")

5.  Eri selainikkunassa Kirjaudu sisään Citrix organisaation Center ([https://account.citrixonline.com/organization/administration/](https://account.citrixonline.com/organization/administration/)).

6. Valitse **Tunnistetietojen toimittaja** -välilehti ja toimi sitten seuraavasti:  

    ![SAML-asetukset] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC6892321.png "SAML-asetukset")

    a. Valitse **Manuaalinen**

    
    b. Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Citrix GoToMeeting** -valintasivun **Kirjautumisen kirjautumissivun URL-osoite** -arvo kopioi ja liitä se sitten **Kirjaudu sisään sivun URL** -tekstiruutuun. 

    
    c-näppäinyhdistelmää. Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Citrix GoToMeeting** -valintasivun **Sign-Out kirjautumissivun URL-osoite** -arvo kopioi ja liitä se sitten **Kirjaudu ulos sivun URL** -tekstiruutuun.

    
    d. Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Citrix GoToMeeting** -valintasivun **Kohteen tunnus** -arvon kopioi ja liitä se sitten **Tunnistetietojen toimittaja kohteen tunnus** -tekstiruutuun.

   
    e. Voit ladata ladatut varmenteen, valitsemalla **Lataa sertifikaatti**.

    
    f. Valitse **Tallenna**.

6.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769000.png "Määritä kertakirjautuminen")


7. Valitse **Sign Vahvista** -sivulla **Valmis**.

    ![SAML-asetukset] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC7689982.png "SAML-asetukset")





##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Active Directory-käyttäjätilien Citrix GoToMeeting valmistelu.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Valitse Azure perinteinen portal-integroinnin **Citrix GoToMeeting** sovellus-sivulla **Määritä käyttäjän valmistelu** -valintaikkunan avaaminen **Määritä käyttäjän valmistelu** .

    ![Määritä käyttäjän valmistelu] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769001.png "Määritä käyttäjän valmistelu")

2.  Valitse **asetukset ja järjestelmänvalvojan tunnistetiedot** -sivulla toimimalla seuraavasti:

    ![Määritä käyttäjän valmistelu] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769002.png "Määritä käyttäjän valmistelu")

    a. Kirjoita järjestelmänvalvojan käyttäjänimi **Citrix GoToMeeting järjestelmänvalvojan käyttäjänimi** -tekstiruutuun.

    
    b. Kirjoita järjestelmänvalvojan salasanan **Citrix GoToMeeting järjestelmänvalvojan salasana** -tekstiruutuun.

    
    c-näppäinyhdistelmää. Valitse **Seuraava**.

3.  Valitse **Vahvista** -sivulla Tallenna kokoonpanosi valintamerkki.

4.  Valitse **Vahvista** -painiketta voit tarkistaa kokoonpanosi.


##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-citrix-gotomeeting-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Citrix GoToMeeting, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Citrix GoToMeeting** sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769003.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC767830.png "Kyllä")

Pitäisi nyt Odota 10 minuuttia ja varmista, että tili on synkronoitu Dropbox for Business.

Vahvistus ensimmäisessä vaiheessa voit tarkistaa valmistelun tilan napsauttamalla Raporttinäkymät-ikkunan d Azure perinteinen portaalin **Citrix GoToMeeting** sovelluksen integrointi-sivulla.

![Raporttinäkymät-ikkunan] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769004.png "Raporttinäkymät-ikkunan")

Jakson valmistelu onnistuneesti suoritettujen käyttäjän ilmaistaan liittyvät tila:

![Integrointitila] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769005.png "Integrointitila")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli.

Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](https://msdn.microsoft.com/library/dn308586).
