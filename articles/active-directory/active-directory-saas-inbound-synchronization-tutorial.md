<properties 
    pageTitle="Opetusohjelma: Määrittäminen työpäivä saapuvien synkronoinnin | Microsoft Azure" 
    description="Opettele käyttämään saapuva synkronoinnin käyttöön kertakirjautumisen, automaattinen valmistelu ja muut Azure Active Directory-hakemistosta!" 
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
    ms.date="04/06/2016" 
    ms.author="jeedes" />

#<a name="tutorial-configuring-workday-for-inbound-synchronization"></a>Opetusohjelma: Määrittäminen työpäivä saapuva synkronointi
>[AZURE.NOTE]Azure Active Directory (AD) Premium on käytettävissä asiakkaille, Kiinassa käyttämällä eri puolilla maailmaa Azure AD-esiintymä.    
Azure AD-Premium ei tällä hetkellä tueta Kiinassa 21vianetin ylläpitämä Microsoft Azure-palvelu.    

Tässä opetusohjelmassa tavoitteena on näyttämään vaiheet, sinun on suoritettava työpäivä ja Microsoft Azure AD henkilöiden tuodaan Microsoft Azure AD työpäivä.    
 Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:  

-   Kelvollinen Azure tilaus  
-   Työpäivä-palvelutili  

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:  

1.  Työpäivä-sovelluksen integrointi ottaminen käyttöön  
2.  Integrointi-Järjestelmäkäyttäjän luominen  
3.  Käyttöoikeusryhmän luominen  
4.  Integrointi Järjestelmäkäyttäjän määritteleminen suojausryhmä  
5.  Suojaus-ryhmän asetusten määrittäminen  
6.  Suojauksen käytännön muutokset aktivoiminen  
7.  Microsoft Azure AD määrittäminen käyttäjän tuonti  

##<a name="enabling-the-application-integration-for-workday"></a>Työpäivä-sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Salesforce-sovelluksen integrointi.    

###<a name="to-enable-the-application-integration-for-workday-perform-the-following-steps"></a>Työpäivä-sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure-hallinta-portaalin vasemmanpuoleisessa siirtymisruudussa.    

    ![Active Directory] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700993.png "Active Directory")  

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.    

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .    

    ![Sovellukset] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700994.png "Sovellukset")  

4.  Avaa **Sovellus valikoima**, **Lisää App**ja valitse sitten **Lisää sovellus tehtävään organisaatiossani, jos haluat käyttää**.    

    ![Mitä haluat tehdä?] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700995.png "Mitä haluat tehdä?")  

5.  Kirjoita **hakuruutuun** **työpäivät**.    

    ![Työpäivä] (./media/active-directory-saas-inbound-synchronization-tutorial/IC701021.png "Työpäivä")  

6.  Valitse tulosruudussa **työpäivä**ja valitse **Valmis** , voit lisätä sovelluksen.    

    ![Työpäivä] (./media/active-directory-saas-inbound-synchronization-tutorial/IC701022.png "Työpäivä")  

##<a name="creating-an-integration-system-user"></a>Integrointi-Järjestelmäkäyttäjän luominen

1.  Kirjoita **Työpäivä Workbench** **Luo käyttäjän** hakukenttään ja napsauttamalla linkkiä, **Luo integrointi järjestelmäkäyttäjä**.     

    ![käyttäjän luominen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750979.png "käyttäjän luominen")  

2.  Luo integrointi Järjestelmäkäyttäjän tehtävän suorittaminen toimittamalla uusi integrointi järjestelmän käyttäjän käyttäjänimi ja salasana.  Jätä edellyttävät uusi salasana seuraavan Kirjaudu sisään valinnan tarkistamatta, koska kyseisen käyttäjän oltava kirjautuminen ohjelmallisesti.    
    Jätä istunnon aikakatkaisu minuuttia sen oletusarvo on 0, joka estää käyttäjän istuntojen aikakatkaisun ennenaikaisesti.    

    ![Integrointi Järjestelmäkäyttäjän luominen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750980.png "Integrointi Järjestelmäkäyttäjän luominen")  

##<a name="creating-a-security-group"></a>Käyttöoikeusryhmän luominen

Tässä opetusohjelmassa kuvatut skenaariossa haluat reunaehtoja integrointi-järjestelmän käyttäjäryhmien luominen ja määritä käyttäjä.    

1.  Kirjoita hakuruutuun käyttäjäryhmien luominen ja valitse sitten Luo käyttöoikeusryhmän-linkin.     

    ![CreateSecurity-ryhmä] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750981.png "CreateSecurity-ryhmä")  

2.  Käyttöoikeusryhmän luominen tehtävän suorittaminen.  Integrointi järjestelmän käyttöoikeusryhmän Valitse – reunaehtoja tyyppi, palveltavaa käyttöoikeusryhmän avattavan luettelon, johon jäsenet erikseen lisätään käyttöoikeusryhmän luominen.     

    ![CreateSecurity-ryhmä] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750982.png "CreateSecurity-ryhmä")  

##<a name="assigning-the-integration-system-user-to-the-security-group"></a>Integrointi Järjestelmäkäyttäjän määritteleminen suojausryhmä

1.  Kirjoita Muokkaa käyttöoikeusryhmän hakuruutuun ja valitse sitten **Muokkaa käyttöoikeusryhmän**-linkin.     

    ![Käyttöoikeusryhmän muokkaaminen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750983.png "Käyttöoikeusryhmän muokkaaminen")  

2.  Etsi ja valitse uusi integrointi käyttöoikeusryhmän nimen mukaan    

    ![Käyttöoikeusryhmän muokkaaminen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750984.png "Käyttöoikeusryhmän muokkaaminen")  

3.  Lisää uusi käyttöoikeusryhmän integrointi järjestelmän uuden käyttäjän.       

    ![Järjestelmän käyttöoikeusryhmän] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750985.png "Järjestelmän käyttöoikeusryhmän")  

##<a name="configuring-security-group-options"></a>Suojaus-ryhmän asetusten määrittäminen

Tässä vaiheessa voit myöntää uuden ryhmän käyttöoikeuksien suojattu toimialueen suojauskäytännöt objektit Hae ja lisää toimintoja:  

-   Ulkoisen tilin valmistelu  
-   Työntekijän tietoja: Julkisen työntekijä raportit  
-   Työntekijän tietoja: Kaikki sijainnit  
-   Työntekijän tietoja: Nykyisen henkilökunta tiedot  
-   Työntekijän tiedot: Business otsikon työntekijän profiili  

&nbsp;  

1.  Kirjoita toimialueen suojauskäytännöt hakuruutuun ja valitse sitten toimialueen suojauskäytäntöjen toiminta-alueen-linkin.     

    ![Toimialueen suojauskäytännöt] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750986.png "Toimialueen suojauskäytännöt")  

2.  Etsi järjestelmä ja valitse järjestelmä-toiminta-alueella.  Napsauta OK merkitä-painiketta.     

    ![Toimialueen suojauskäytännöt] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750987.png "Toimialueen suojauskäytännöt")  

3.  Laajenna suojauksen etähallinta järjestelmän toiminta-alueella suojauskäytäntöjä-luettelosta ja valitse toimialueen suojauskäytäntö ulkoisen tilin valmistelu.     

    ![Toimialueen suojauskäytännöt] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750988.png "Toimialueen suojauskäytännöt")  

4.  Napsauta Muokkaa käyttöoikeuksia-painiketta ja sitten Muokkaa käyttöoikeuksia-näytössä Lisää uusi käyttöoikeusryhmän luetteloon suojausryhmien Hae ja lisää integrointi käyttöoikeuksien avulla.     

    ![Muokkaa käyttöoikeuksia] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750989.png "Muokkaa käyttöoikeuksia")  

5.  Toista vaihe 1, yläpuolella, voit palauttaa näytön valitsemiseen toiminta-alueet, ja tällä hetkellä hakeminen henkilökunta, valitse henkilöstön palkkaamisesta vastaavan toiminta-alueella ja valitse merkitä OK-painiketta.    

    ![Toimialueen suojauskäytännöt] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750990.png "Toimialueen suojauskäytännöt")  

6.  Laajenna henkilöstön palkkaamisesta vastaavan toiminta-alueella suojauskäytäntöjä luettelo työntekijän tietoja: henkilöstön palkkaamisesta vastaavan ja toista vaihe 4 yllä kaikkien näiden jäljellä suojauskäytäntöjä:    

    -   Työntekijän tietoja: Julkisen työntekijä raportit  
    -   Työntekijän tietoja: Kaikki sijainnit  
    -   Työntekijän tietoja: Nykyisen henkilökunta tiedot  
    -   Työntekijän tiedot: Business otsikon työntekijän profiili    

    ![Toimialueen suojauskäytännöt] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750991.png "Toimialueen suojauskäytännöt")  

##<a name="activating-security-policy-changes"></a>Suojauksen käytännön muutokset aktivoiminen

1.  Kirjoita hakuruutuun aktivoiminen ja valitse sitten Aktivoi odottavat suojauksen käytännön muutokset-linkin.    

    ![Aktivoiminen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750992.png "Aktivoiminen")  

2.   Aloita aktivointi odottavat suojauksen käytännön muutokset tehtävän antamalla kommentin valvonta tarkoituksiin ja valitsemalla sitten OK merkitä-painikkeen.      

    ![Aktivoi odottavat suojaus] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750993.png "Aktivoi odottavat suojaus")  

3.  Valitse seuraavassa näytössä suorittaminen valitsemalla Vahvista ja valitsemalla OK merkitä-painikkeen valintaruudun valinta.     

    ![Aktivoi odottavat suojaus] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750994.png "Aktivoi odottavat suojaus")  

##<a name="configuring-user-import-in-microsoft-azure-ad"></a>Microsoft Azure AD määrittäminen käyttäjän tuonti

Tässä osassa tavoitteena on jäsentäminen Microsoft Azure AD henkilöiden tuomisesta työpäivä määrittämisestä.    

###<a name="to-configure-user-import-in-microsoft-azure-ad-perform-the-following-steps"></a>Voit määrittää käyttäjän tuominen Microsoft Azure AD toimimalla seuraavasti:

1.  Integrointi **työpäivä** sovellus-sivulla Valitse **Määritä käyttäjän tuo** **Määrittäminen valmistelu** -valintaikkunan avaaminen.    

2.  Valitse **asetukset ja järjestelmänvalvojan tunnistetiedot** -sivulla suorittamalla seuraavat vaiheet ja valitse sitten Seuraava:    

    ![Asetukset ja järjestelmänvalvojan tunnistetietoja] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750995.png "Asetukset ja järjestelmänvalvojan tunnistetietoja")    

    1.  Kirjoita **työpäivä järjestelmänvalvojan käyttäjänimi** -tekstiruutuun olet luonut [integrointi-Järjestelmäkäyttäjän luominen](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) -kohdassa käyttäjän nimi.    
    2.  Kirjoita olet luonut [integrointi-Järjestelmäkäyttäjän luominen](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) -kohdassa käyttäjän salasanan **työpäivä järjestelmänvalvojan salasana** -tekstiruutuun.    
    3.  Kirjoita URL-osoite tai työpäivä-vuokraajan **työpäivä vuokraajan URL** -tekstiruutuun.    

3.  Valitse **Testaa yhteys** -sivulla valitsemalla **Aloita testi** Vahvista yhteys ja valitse sitten **Seuraava**.    

    ![Testiyhteys] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750996.png "Testiyhteys")  

4.  Valitse **Provisioning asetukset** -sivulla **Seuraava**.    

    ![Valmistelu asetukset] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750997.png "Valmistelu asetukset")  

5.  Valitse **Käynnistä valmistelu** -valintaikkunassa valitsemalla **Valmis**.    

    ![Käynnistä valmistelu] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750998.png "Käynnistä valmistelu")  

Voit nyt **käyttäjät** -kohdassa ja tarkista, onko työpäivä-käyttäjä on tuotu.    
