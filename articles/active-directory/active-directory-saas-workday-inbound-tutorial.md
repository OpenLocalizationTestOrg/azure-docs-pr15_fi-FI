<properties 
    pageTitle="Opetusohjelma: Määrittäminen työpäivä saapuvien synkronoinnin | Microsoft Azure" 
    description="Opi käyttämään työpäivä käyttäjätiedot lähteen Azure Active Directory." 
    services="active-directory" 
    authors="MarkusVi"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/10/2016" 
    ms.author="markvi" />


#<a name="tutorial-configuring-workday-for-inbound-synchronization"></a>Opetusohjelma: Määrittäminen työpäivä saapuva synkronointi


Tässä opetusohjelmassa tavoitteena on näyttämään vaiheet, sinun on suoritettava työpäivä ja Azure AD henkilöiden tuodaan Azure AD työpäivä. 

Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure AD Premium-tilaus
-   Työpäivä-palvelutili
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1. Työpäivä-sovelluksen integrointi ottaminen käyttöön 


2. Integrointi-Järjestelmäkäyttäjän luominen 


3. Käyttöoikeusryhmän luominen 


4. Integrointi Järjestelmäkäyttäjän määritteleminen suojausryhmä 


5. Suojaus-ryhmän asetusten määrittäminen 


6. Suojauksen käytännön muutokset aktivoiminen 


7. Määritettäessä Azure AD-käyttäjän tuonti 



##<a name="enabling-the-application-integration-for-workday"></a>Työpäivä-sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön työpäivä sovelluksen integrointi.

### <a name="steps"></a>Ohjeita:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-workday-inbound-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-workday-inbound-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-workday-inbound-tutorial/IC749321.png "Lisää sovellus")

  
5. Kirjoita hakuruutuun **työpäivät**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-workday-inbound-tutorial/IC701021.png "Lisää sovellus-gallerry")

6. Valitse tulokset-ruudussa työpäivä ja valitse Valmis, voit lisätä sovelluksen.

    ![Sovelluksen valikoima] (./media/active-directory-saas-workday-inbound-tutorial/IC701022.png "Sovelluksen valikoima")





## <a name="creating-an-integration-system-user"></a>Integrointi-Järjestelmäkäyttäjän luominen

### <a name="steps"></a>Ohjeita:


1. Kirjoita **Työpäivä Workbench**Luo käyttäjän hakukenttään ja valitse sitten **Luo integrointi järjestelmäkäyttäjä**. 

    ![Käyttäjän luominen] (./media/active-directory-saas-workday-inbound-tutorial/IC750979.png "Käyttäjän luominen")



2. **Luo integrointi Järjestelmäkäyttäjän** tehtävän suorittaminen toimittamalla uusi integrointi järjestelmän käyttäjän käyttäjänimi ja salasana.  Jätä edellyttävät uusi salasana seuraavan Kirjaudu sisään valinnan tarkistamatta, koska kyseisen käyttäjän oltava kirjautuminen ohjelmallisesti. Jätä istunnon aikakatkaisu minuuttia sen oletusarvo on 0, joka estää käyttäjän istuntojen aikakatkaisun ennenaikaisesti. 

    ![Integrointi Järjestelmäkäyttäjän luominen] (./media/active-directory-saas-workday-inbound-tutorial/IC750980.png "Integrointi Järjestelmäkäyttäjän luominen")
 




## <a name="creating-a-security-group"></a>Käyttöoikeusryhmän luominen

Tässä opetusohjelmassa kuvatut skenaariossa haluat reunaehtoja integrointi-järjestelmän käyttäjäryhmien luominen ja määritä käyttäjä.

### <a name="steps"></a>Ohjeita:

1. Kirjoita hakuruutuun käyttäjäryhmien luominen ja valitse sitten **Luo käyttöoikeusryhmän**. 

    ![CreateSecurity-ryhmä] (./media/active-directory-saas-workday-inbound-tutorial/IC750981.png "CreateSecurity-ryhmä")
 

2. Käyttöoikeusryhmän luominen tehtävän suorittaminen.  Integrointi järjestelmän käyttöoikeusryhmän Valitse – reunaehtoja tyyppi, palveltavaa käyttöoikeusryhmän avattavan luettelon, johon jäsenet erikseen lisätään käyttöoikeusryhmän luominen. 

    ![CreateSecurity-ryhmä] (./media/active-directory-saas-workday-inbound-tutorial/IC750982.png "CreateSecurity-ryhmä")
 



## <a name="assigning-the-integration-system-user-to-the-security-group"></a>Integrointi Järjestelmäkäyttäjän määritteleminen suojausryhmä

### <a name="steps"></a>Ohjeita:


1. Kirjoita Muokkaa käyttöoikeusryhmän hakuruutuun ja valitse sitten **Muokkaa käyttöoikeusryhmän**. 

    ![Käyttöoikeusryhmän muokkaaminen] (./media/active-directory-saas-workday-inbound-tutorial/IC750983.png "Käyttöoikeusryhmän muokkaaminen")
 
 

2. Etsi ja valitse uusi integrointi suojaus-ryhmä nimen mukaan. 

    ![Käyttöoikeusryhmän muokkaaminen] (./media/active-directory-saas-workday-inbound-tutorial/IC750984.png "Käyttöoikeusryhmän muokkaaminen")

 

3. Lisää uusi käyttöoikeusryhmän integrointi järjestelmän uuden käyttäjän. 

    ![Järjestelmän käyttöoikeusryhmän] (./media/active-directory-saas-workday-inbound-tutorial/IC750985.png "Järjestelmän käyttöoikeusryhmän")  




## <a name="configuring-security-group-options"></a>Suojaus-ryhmän asetusten määrittäminen

Tässä vaiheessa voit myöntää uuden ryhmän käyttöoikeuksien **hankkiminen** ja **siirtää** suojattu toimialueen suojauskäytännöt objektit toimenpiteet:

- Ulkoisen tilin valmistelu

- Työntekijän tietoja: Julkisen työntekijä raportit

- Työntekijän tietoja: Kaikki sijainnit

- Työntekijän tietoja: Nykyisen henkilökunta tiedot

- Työntekijän tiedot: Business otsikon työntekijän profiili

 
### <a name="steps"></a>Ohjeita:

1. Kirjoita toimialueen suojauskäytännöt hakuruutuun ja valitse sitten toimialueen suojauskäytäntöjen toiminta-alueen-linkin.  

    ![Toimialueen suojauskäytännöt] (./media/active-directory-saas-workday-inbound-tutorial/IC750986.png "Toimialueen suojauskäytännöt")  
 

2. Etsi järjestelmä ja valitse **Järjestelmä** -toiminta-alueella.  Valitse **OK**.  

    ![Toimialueen suojauskäytännöt] (./media/active-directory-saas-workday-inbound-tutorial/IC750987.png "Toimialueen suojauskäytännöt")  


3. Laajenna suojauksen etähallinta järjestelmän toiminta-alueella suojauskäytäntöjä-luettelosta ja valitse toimialueen suojauskäytäntö ulkoisen tilin valmistelu.  

    ![Toimialueen suojauskäytännöt] (./media/active-directory-saas-workday-inbound-tutorial/IC750988.png "Toimialueen suojauskäytännöt")  


4. **Muokkaa**käyttöoikeuksia ja lisää sitten uusi käyttöoikeusryhmän **Muokkaa käyttöoikeuksia**-sivulla **Hae** muokkausoikeudet ja **sijoittaa** suojausryhmien luetteloon. 

    ![Muokkaa käyttöoikeuksia] (./media/active-directory-saas-workday-inbound-tutorial/IC750989.png "Muokkaa käyttöoikeuksia")  

 

5. Toista vaihe 1, yläpuolella, voit palauttaa näytön valitsemiseen toiminta-alueet, ja tällä hetkellä hakeminen henkilökunta, valitse henkilöstön palkkaamisesta vastaavan toiminta-alueella ja valitse **OK**.

    ![Toimialueen suojauskäytännöt] (./media/active-directory-saas-workday-inbound-tutorial/IC750990.png "Toimialueen suojauskäytännöt")  
 

6. Laajenna henkilöstön palkkaamisesta vastaavan toiminta-alueella suojauskäytäntöjä luettelo työntekijän tietoja: henkilöstön palkkaamisesta vastaavan ja toista vaihe 4 yllä kaikkien näiden jäljellä suojauskäytäntöjä:

     - Työntekijän tietoja: Julkisen työntekijä raportit

     - Työntekijän tietoja: Kaikki sijainnit

     - Työntekijän tietoja: Nykyisen henkilökunta tiedot

     - Työntekijän tiedot: Business otsikon työntekijän profiili


    ![Toimialueen suojauskäytännöt] (./media/active-directory-saas-workday-inbound-tutorial/IC750991.png "Toimialueen suojauskäytännöt")  







## <a name="activating-security-policy-changes"></a>Suojauksen käytännön muutokset aktivoiminen

### <a name="steps"></a>Ohjeita:

1. Kirjoita hakuruutuun aktivoiminen ja valitse sitten Aktivoi odottavat suojauksen käytännön muutokset-linkin. 

    ![Aktivoiminen] (./media/active-directory-saas-workday-inbound-tutorial/IC750992.png "Aktivoiminen") 
 

2. Aloita aktivointi odottavat suojauksen käytännön muutokset tehtävän kirjoittamalla valvonta tarkoituksiin kommentin, ja valitse sitten **OK**. 

    ![Aktivoi odottavat suojaus] (./media/active-directory-saas-workday-inbound-tutorial/IC750993.png "Aktivoi odottavat suojaus")   
 

3. Valitse seuraavassa näytössä tehtävän suorittaminen valitsemalla merkitä Vahvista valintaruutu ja valitse sitten **OK**. 

    ![Aktivoi odottavat suojaus] (./media/active-directory-saas-workday-inbound-tutorial/IC750994.png "Aktivoi odottavat suojaus")  





## <a name="configuring-user-import-in-azure-ad"></a>Määritettäessä Azure AD-käyttäjän tuonti

Tässä osassa tavoitteena on jäsentäminen Azure AD henkilöiden tuomisesta työpäivä määrittämisestä.


### <a name="steps"></a>Ohjeita:


1. Integrointi **työpäivä** sovellus-sivulla Valitse **Määritä käyttäjän tuo** **Määrittäminen valmistelu** -valintaikkunan avaaminen.


2. Valitse **asetukset ja järjestelmänvalvojan tunnistetiedot** -sivulla suorittamalla seuraavat vaiheet ja valitse sitten **Seuraava**: 

    ![Asetukset ja järjestelmänvalvojan tunnistetietoja] (./media/active-directory-saas-workday-inbound-tutorial/IC750995.png "Asetukset ja järjestelmänvalvojan tunnistetietoja")  
 
    a. Kirjoita työpäivä järjestelmänvalvojan käyttäjän nimi-tekstiruutuun luominen luomasi käyttäjän nimi integrointi järjestelmän käyttäjä-osassa.

    b. Kirjoita työpäivä järjestelmänvalvojan salasana-tekstiruutuun luominen luomasi käyttäjän salasanan integrointi järjestelmän käyttäjä-osassa.

    c-näppäinyhdistelmää. Kirjoita URL-osoite tai työpäivä-vuokraajan työpäivä vuokraajan URL-tekstiruutuun.


3. Valitse **Testaa yhteys** -sivulla valitsemalla **Aloita testi** Vahvista yhteys ja valitse sitten **Seuraava**. 

    ![Testiyhteys] (./media/active-directory-saas-workday-inbound-tutorial/IC750996.png "Testiyhteys")  
 

4. Valitse **Provisioning** asetukset-sivulla **Seuraava**. 

    ![Valmistelu asetukset] (./media/active-directory-saas-workday-inbound-tutorial/IC750997.png "Valmistelu asetukset")


5. Valitse **Käynnistä valmistelu** -valintaikkunassa valitsemalla **Valmis**. 

    ![Käynnistä valmistelu] (./media/active-directory-saas-workday-inbound-tutorial/IC750998.png "Käynnistä valmistelu")
 

Voit nyt **käyttäjät** -kohdassa ja tarkista, onko työpäivä-käyttäjä on tuotu.



## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)
