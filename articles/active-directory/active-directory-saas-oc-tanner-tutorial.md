<properties
    pageTitle="Opetusohjelma: O.C. Azure Active Directory-integrointi Virtanen - AppreciateHub | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja O.C. välillä Virtanen - AppreciateHub."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-oc-tanner---appreciatehub"></a>Opetusohjelma: O.C. Azure Active Directory-integrointi Virtanen - AppreciateHub

Tässä opetusohjelmassa tavoitteena on kerrotaan, miten voit integroida O.C. Virtanen - AppreciateHub ja Azure Active Directory (Azure AD).  
Paikallisen ympäristön integroinnissa O.C. Virtanen - AppreciateHub Azure AD kanssa löytyy seuraavat edut: 

- Voit hallita Azure AD, jolla on pääsy O.C. Virtanen - AppreciateHub 
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön O.C. käyttäjille Virtanen - AppreciateHub (kertakirjautumisen) niiden Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset 

Määrittää O.C. Azure AD-integrointi Virtanen - AppreciateHub, tarvitset seuraavat:

- Azure AD-tilaus
- O.C. Virtanen - AppreciateHub single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.  
Tässä opetusohjelmassa kuvatut skenaarion koostuu kolme tärkeimmät rakenneosien:

1. O.C. lisääminen Virtanen - AppreciateHub valikoimasta 
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-oc-tanner---appreciatehub-from-the-gallery"></a>O.C. lisääminen Virtanen - AppreciateHub valikoimasta
Jos haluat määrittää O.C. integrointi Virtanen - AppreciateHub suoraan Azure AD, sinun on lisättävä O.C. Virtanen - valikoimasta AppreciateHub hallitun SaaS sovellusten luetteloon.

**Voit lisätä O.C. Virtanen - AppreciateHub valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1] 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2] 

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3] 

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4] 

6. Kirjoita hakuruutuun **O.C. Virtanen - AppreciateHub**.

    ![Sovellukset][5] 

7. Valitse tulosruudussa **O.C. Virtanen - AppreciateHub**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Sovellukset][25] 




##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on

Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen O.C. kanssa Virtanen - nimeltä "Britta Simon" testi käyttäjään perustuva AppreciateHub.

Kertakirjautumisen toimimaan, saat Azure AD tarvitsee tietää, mitä vastaavaan käyttäjän O.C. Virtanen - AppreciateHub Azure AD-käyttäjälle on. Toisin sanoen linkki Azure AD-käyttäjä ja Aiheeseen liittyvät-O.C. välisen suhteen Virtanen - AppreciateHub on vahvistettava.  
Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi O.C. **käyttäjänimi** Azure AD Virtanen - AppreciateHub.
 
Määritä ja testaa Azure AD kertakirjautumisen O.C. kanssa Virtanen - AppreciateHub, sinun on suoritettava seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Luodaan O.C. Virtanen - AppreciateHub testikäyttäjän](#creating-a-halogen-software-test-user)** - antaaksesi tapahtumista Britta Simon O.C. Virtanen - AppreciateHub, jotka on linkitetty hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa on Azure AD kertakirjautumisen Azure perinteinen portaalissa käyttöön ja määrittäminen oman O.C. kertakirjautumisen Virtanen - AppreciateHub sovelluksen.


**Määrittää Azure AD kertakirjautumisen O.C. Virtanen - AppreciateHub, toimi seuraavasti:**

1. Valitse Azure perinteinen portaalissa integrointi **O.C. Virtanen - AppreciateHub** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][6]

2. **Miten käyttäjät voivat kirjautua O.C. Virtanen - AppreciateHub haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][7]

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:

    ![Sovelluksen asetusten määrittäminen][8]
 
     a. Avaa metatietojen tiedoston käyttämällä seuraavaa linkkiä: [https://fed.appreciatehub.com/fed/sp/metadata](https://fed.appreciatehub.com/fed/sp/metadata).

     b. Etsi solmu **md:AssertionConsumerService** . 

     c-näppäinyhdistelmää. Kopioi **sijainti** -määritteen arvon. 

     ![Sovelluksen asetusten määrittäminen][12]
     
     d. Kirjoita **Merkki-URL** -tekstiruutuun aiempia arvon olet saanut edellisessä vaiheessa.

     > [AZURE.NOTE] Jos olet expiriencing ongelmia vastaa URL-Osoitteen metatiedot-tiedostosta, ota yhteyttä O.C. Virtanen - AppreciateHub tukihenkilöstön kautta [sso@octanner.com](mailto:sso@octanner.com).

     e. Valitse **Seuraava**.
 
4. Valitse **Määritä kertakirjautumisen osoitteessa O.C. Virtanen - AppreciateHub** -sivulla valitsemalla **Lataa metatiedot**ja Tallenna paikallisesti tietokoneen metatiedot-tiedostoon.

    ![Mikä on Azure AD Connect][9]

5. Yhteyshenkilö O.C. Virtanen - AppreciateHub tukihenkilöstön xyz, kautta antaa niille metatieto-tiedosto, ja ne ilmoita siitä, että ne Ota SSO puolestasi.


6. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**. 

    ![Mikä on Azure AD Connect][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  

    ![Mikä on Azure AD Connect][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.  

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_02.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_03.png) 
 
4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**. 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti: 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet: 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_06.png)
 
    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.
    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_07.png) 
 
8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_08.png) 
  
    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   

  
 
### <a name="creating-a-oc-tanner---appreciatehub-test-user"></a>O.C. luominen Virtanen - AppreciateHub testikäyttäjän

Tässä osassa tavoitteena on nimeltään Britta Simon O.C. käyttäjän luominen Virtanen - AppreciateHub.

**Luo käyttäjä kutsutaan Britta Simon O.C. Virtanen - AppreciateHub, toimi seuraavasti:**

1. Pyydä OC Virtanen tukihenkilöstölle Luo käyttäjä, jolla on kuin nameID Azure AD-määrite saman arvon kuin Britta Simon käyttäjänimi.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä O.C. ottaminen käyttöön Virtanen - AppreciateHub.

![Määrittää käyttäjälle][200]

**Jos haluat määrittää Britta Simon O.C. Virtanen - AppreciateHub, toimimalla seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201]

2. Valitse sovellukset-luettelosta **O.C. Virtanen - AppreciateHub**.

    ![Määrittää käyttäjälle][202]

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203]

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.  
Kun napsautat O.C. Virtanen - AppreciateHub ruutu Access-toiminnolla voit pitäisi saada automaattisesti kirjautunut käyttöön oman O.C. Virtanen - AppreciateHub sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->
[1]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_01.png
[25]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_25.png


[6]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_02.png
[8]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_03.png
[9]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_04.png
[10]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_05.png
[11]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_06.png
[12]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_08.png
[20]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_07.png
[203]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_205.png






