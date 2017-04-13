<properties
    pageTitle="Access-Ohjauspaneelin osa ottamisesta käyttöön Internet Explorerin ryhmäkäytännön avulla | Microsoft Azure"
    description="Voit ottaa käyttöön Internet Explorer-lisäosan Omat sovellukset-portaalin ryhmäkäytännön avulla."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/16/2016"
    ms.author="markvi"/>

#<a name="how-to-deploy-the-access-panel-extension-for-internet-explorer-using-group-policy"></a>Access-Ohjauspaneelin osa ottamisesta käyttöön Internet Explorerin ryhmäkäytännön avulla

Tässä opetusohjelmassa kerrotaan, miten etäasennuksen Access Ohjauspaneelin osa Internet Explorerin käyttäjien tietokoneissa ryhmäkäytännön avulla. Tämän tunnisteen tarvitaan Internet Explorer-käyttäjät, joilla on kirjauduttava sisään sovellukset, jotka on määritetty [salasana-pohjainen kertakirjautumisen](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)avulla.

On suositeltavaa järjestelmänvalvojat automatisoida tämän tunnisteen käyttöönotto. Muussa tapauksessa käyttäjät on Lataa ja asenna laajennus itse, mitkä ei voi enää käyttäjän virhe ja edellyttää järjestelmänvalvojan oikeuksia. Tässä opetusohjelmassa kerrotaan yhden keinoja niiden automatisointiin ohjelmiston ominaisuuksissa ryhmäkäytännön avulla. [Lisätietoja Ryhmäkäytäntö.](https://technet.microsoft.com/windowsserver/bb310732.aspx)

Access-Ohjauspaneelin osa on myös käytettävissä [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) ja [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), joista kumpikaan ei edellytä järjestelmänvalvojan oikeudet asentaa.

##<a name="prerequisites"></a>Edellytykset

- Olet määrittänyt [Active Directory-toimialueen palveluista](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx)ja olet liittynyt käyttäjien koneet toimialueeseen.
- Tarvitset halutaan muuttaa ryhmäkäytännön ryhmäkäytäntöobjekteja "Muokkaa asetuksia"-käyttöoikeudet. Oletusarvon mukaan seuraavien suojausryhmien jäsenillä on nämä käyttöoikeudet: toimialueen Järjestelmänvalvojat, yrityksen Järjestelmänvalvojat ja ryhmäkäytäntöjen luoja-omistaja. [Opi lisää.](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

##<a name="step-1-create-the-distribution-point"></a>Vaihe 1: Luo jakauman-kohta

Ensin sinun täytyy lisätä asennuspaketti verkkosijainnissa, niitä voi käyttää kaikkien koneet, josta haluat etäasennuksen tunniste. Voit tehdä tämän seuraavasti:

1. Kirjaudu sisään järjestelmänvalvojan palvelimeen

2. Siirry **Palvelimen hallinta** -ikkunassa **tiedostoja ja tallennustilaa palveluja**.

    ![Tiedostojen avaaminen ja tallennustilaa palvelut](./media/active-directory-saas-ie-group-policy/files-services.png)

3. Avaa **Jaetut resurssit** -välilehti. Valitse **tehtävien** > **Uusi Jaa...**

    ![Tiedostojen avaaminen ja tallennustilaa palvelut](./media/active-directory-saas-ie-group-policy/shares.png)

4. **Jaa ohjattu** ja määrittää käyttöoikeuksia, varmista, että se voidaan käyttää käyttäjien tietokoneissa. [Lisätietoja osakkeet.](https://technet.microsoft.com/library/cc753175.aspx)

5. Lataa seuraavat Microsoft Windows Installer-paketti (.msi-tiedostoa): [Access-paneelin Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)

6. Kopioi asennuspaketti haluamaasi kohtaan, valitse Jaa.

    ![Kopioi .msi-tiedostoa, jonka olet Jaa.](./media/active-directory-saas-ie-group-policy/copy-package.png)

8. Tarkista, että asiakaskoneiden voi käyttää asennuspaketti Jaa. 

##<a name="step-2-create-the-group-policy-object"></a>Vaihe 2: Luo ryhmäkäytäntöobjekti

1. Kirjaudu Active Directory-toimialueen palveluista (AD DS) asennuksen isännöivän palvelimen.

2. Palvelimen hallinnan valitsemalla **Työkalut** > **Ryhmäkäytäntöjen hallinta**.

    ![Valitse Työkalut > Ryhmittele käytännön Management](./media/active-directory-saas-ie-group-policy/tools-gpm.png)

3. **Ryhmäkäytäntöjen hallinta** -ikkunan vasemmanpuoleisessa ruudussa organisaatioyksikkö (OU) hierarkian tarkastelemista ja selvittää, mitkä alueessa haluat käyttää ryhmäkäytännön. Esimerkiksi voit päättää, josta voi valita käyttöön muutamalla käyttäjällä testauksessa pieni OU tai voi valita ylimmän tason OU koko organisaation käyttöön.

    > [AZURE.NOTE] Jos haluat luoda tai muokata organisaatioyksiköiden (organisaatioyksiköiden), Vaihda takaisin Server Manageriin ja **Työkalut** > **Active Directoryn käyttäjät ja tietokoneissa**.

4. Kun olet valinnut OU, napsauttamalla sitä hiiren kakkospainikkeella ja valitse **Luo tämän toimialueen Ryhmäkäytäntöobjekti ja linkitä se tässä...**

    ![Luo uusi Ryhmäkäytäntöobjekti](./media/active-directory-saas-ie-group-policy/create-gpo.png)

5. Kirjoita **Uusi Ryhmäkäytäntöobjekti** , kehotteen uusi ryhmäkäytäntöobjekti nimi.

    ![Nimeä uusi Ryhmäkäytäntöobjekti](./media/active-directory-saas-ie-group-policy/name-gpo.png)

6. Valitse juuri luomasi ryhmäkäytäntöobjekti hiiren kakkospainikkeella ja valitse **Muokkaa**.

    ![Uusi Ryhmäkäytäntöobjekti muokkaaminen](./media/active-directory-saas-ie-group-policy/edit-gpo.png)

##<a name="step-3-assign-the-installation-package"></a>Vaihe 3: Määritä asennuspaketin

1. Määrittää, haluatko ottaa käyttöön **Tietokoneen määritykset** tai **Käyttäjäasetukset**tunniste. Kun käytät [tietokoneen määritykset](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), tunniste on asennetaan riippumatta siitä, mitä käyttäjät kirjautuvat se tietokoneeseen. Toisaalta [Käyttäjäasetukset](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), käyttäjien on asennettu niiden riippumatta siitä, mitä tietokoneita he kirjautuvat tunniste.

2. Valitse **Ryhmäkäytäntöjen hallinnan editorin** -ikkunan vasemmanpuoleisessa ruudussa jompikumpi seuraavista kansio-poluista valitsit määritysten tyypin mukaan:
    - `Computer Configuration/Policies/Software Settings/`
    - `User Configuration/Policies/Software Settings/`

3. Napsauta **Ohjelmistoasennus**hiiren kakkospainikkeella ja valitse sitten **Uusi** > **paketti...**

    ![Luo uusi ohjelmiston asennuspaketin](./media/active-directory-saas-ie-group-policy/new-package.png)

4. Siirry jaettuun kansioon, joka sisältää-asennuspaketti [Vaihe 1: Luo Distribution Point](#step-1-create-the-distribution-point).msi-tiedostoa ja valitse sitten **Avaa**.

    > [AZURE.IMPORTANT] Jos Jaa sijaitsee tässä samassa palvelimessa, tarkista, kun käytät .msi – verkon tiedoston koko polku paikallisen tiedoston sijaan.

    ![Valitse asennuspaketin jaettuun kansioon.](./media/active-directory-saas-ie-group-policy/select-package.png)

5. Valitse **Ohjelmiston käyttöönotto** -kehotteessa **määritetyt** käyttöönotto-menetelmää. Valitse **OK**.

    ![Valitse Vastuuhenkilö ja valitse sitten OK.](./media/active-directory-saas-ie-group-policy/deployment-method.png)

Tiedostotunniste on nyt otettu käyttöön, jonka valitsit OU. [Lisätietoja Ohjelmistoasennus.](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

##<a name="step-4-auto-enable-the-extension-for-internet-explorer"></a>Vaihe 4: Automaattisen selainkäyttöominaisuudet käyttöön Internet Explorer-laajennus 

Suorittamalla asennusohjelma jokaisen Internet Explorer-laajennus on otettava erikseen käyttöön ennen kuin sitä voidaan käyttää. Noudata seuraavia ohjeita voit ottaa käyttöön Access-Ohjauspaneelin osa ryhmäkäytännön avulla:

1. Valitse **Ryhmäkäytäntöjen hallinnan editorin** -ikkunassa jompikumpi seuraavista poluista-tyypin määritysten mukaan valitsit- [Vaihe 3: Määritä asennuspaketin](#step-3-assign-the-installation-package):
    - `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
    - `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`

2. **Lisäosien**luettelossa hiiren kakkospainikkeella ja valitse **Muokkaa**.
    ![Muokkaa luetteloa lisäosa.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)

3. **Lisäosien luettelo** -ikkunassa valitsemalla **käytössä**. Valitse **asetukset** -kohdassa Valitse **Näytä...**.

    ![Valitse Ota käyttöön ja valitse sitten Näytä...](./media/active-directory-saas-ie-group-policy/edit-add-on-list-window.png)

4. **Näytä sisältö** -ikkunassa toimi seuraavasti:

    1. Ensimmäisen sarakkeen (kentän **Arvon nimi** ) kopioi ja liitä seuraava Luokkatunnus:`{030E9A3F-7B18-4122-9A60-B87235E4F59E}`

    2. Toisessa sarakkeessa **(arvokentän)** Kirjoita seuraava arvo:`1`

    3. Valitse **OK** ja sulje **Näytä sisältö** -ikkunassa.

    ![Täytä arvot edellä määritetyllä tavalla.](./media/active-directory-saas-ie-group-policy/show-contents.png)

5. Valitse Ota muutokset käyttöön ja sulje **Lisäosaluettelo** -ikkunassa **OK** .

Tunniste on nyt ottaa käyttöön valitun OU tietokoneissa. [Lisätietoja ottaminen käyttöön ja poista Internet Explorerin lisäosat käytöstä ryhmäkäytännön avulla.](https://technet.microsoft.com/library/dn454941.aspx)

##<a name="step-5-optional-disable-remember-password-prompt"></a>Vaihe 5 (valinnainen): Poista käytöstä "Muista salasana-kehote

Kun käyttäjät kirjautua sisään sivustojen käyttämällä Access-Ohjauspaneelin osa, Internet Explorer saattaa näkyä seuraava kehote, jossa kysytään, "Haluatko tallentaa salasanan?"

![](./media/active-directory-saas-ie-group-policy/remember-password-prompt.png)

Jos haluat estää käyttäjiä näkemästä tämän kehotteen, noudata seuraavia ohjeita voit estää automaattisen täydennyksen poistaminen salasanojen muistaminen:

1. Siirry **Ryhmäkäytäntöjen hallinnan editorin** -ikkunassa alla polku. Huomaa, että kokoonpano-asetus on käytettävissä vain **Käyttäjäasetukset**.
    - `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`

2. Etsi asetus nimeltä **automaattisen täydennyksen näkyvä, käyttäjänimet ja salasanat**.

    > [AZURE.NOTE] Active Directory aiempien versioiden saattaa luettelon asetus **Älä salli automaattisen täydennyksen tallentamaan salasanat**nimellä. Siten, että se ei ole sama kuin tässä opetusohjelmassa kuvataan asetus.

    ![Muista, tarkista tämä asetukset-kohdassa.](./media/active-directory-saas-ie-group-policy/disable-auto-complete.png)

3. Napsauta yllä asetus hiiren kakkospainikkeella ja valitse **Muokkaa**.

4. -Ikkunassa liittyvä **käyttäjänimet ja salasanat automaattisen täydennyksen-toiminnon käyttöön**Valitse **ei käytössä**.

    ![Valitse Poista käytöstä](./media/active-directory-saas-ie-group-policy/disable-passwords.png)

5. Valitse **OK** , jos haluat ottaa muutokset käyttöön ja sulje ikkuna.

Käyttäjät eivät enää voi tallentaa niiden tunnistetiedot tai automaattisen täydennyksen käyttäminen, voit käyttää aiemmin tallennettuja tunnistetietoja. Kuitenkin tämän käytännön avulla käyttäjät voivat edelleen käyttää muuntyyppisten lomakekentät, kuten haun kenttien automaattisen täydennyksen.

> [AZURE.WARNING] Jos tämä käytäntö on otettu käyttöön, kun käyttäjät ovat päättäneet käytännön tässä on joitakin tunnistetietojen tallentaminen *ei* Poista tunnistetiedot, jotka on jo tallennettu.

##<a name="step-6-testing-the-deployment"></a>Vaihe 6: Käyttöönoton testaaminen

Noudata seuraavia ohjeita vahvistamiseksi, jos tunniste-käyttöönoton onnistui:

1. Jos olet asentanut käyttämällä **Tietokoneen määritykset**, kirjaudu sisään asiakaskoneeseen, joihin kuuluu, jonka valitsit OU [Vaihe 2: Luo ryhmäkäytäntöobjekti](#step-2-create-the-group-policy-object). Jos olet asentanut käyttämällä **Käyttäjäasetukset**, varmista, että kirjautua sisään käyttäjänä, jolla kyseisen OU kuuluu.

2. Voi kestää muutaman Kirjaudu ins ryhmäkäytännön muuttuu täysin Päivitä tämän tietokoneen kanssa. Pakota päivitys, Avaa **Komentorivi** -ikkuna ja suorittamalla seuraavan komennon:`gpupdate /force`

3. Sinun on käynnistämään tietokoneen asennuksen asetetaan. Bootup voi kestää huomattavasti kauemmin kuin tavanomainen aikana tunniste on asennettu.

4. Kun olet käynnistänyt, Avaa **Internet Explorer**. Valitse ikkunan oikeassa yläkulmassa valitsemalla **Työkalut** (hammaspyöräkuvake) ja valitse sitten **Lisäosien hallinta**.

    ![Valitse Työkalut > Lisäosien hallinta](./media/active-directory-saas-ie-group-policy/manage-add-ons.png)

5. **Lisäosien hallinta** -ikkunassa Tarkista, että **Access Ohjauspaneelin osa** on asennettu ja sen **tila** on otettu **käyttöön**.

    ![Varmista, että Access Ohjauspaneelin osa on asennettu ja otettu käyttöön.](./media/active-directory-saas-ie-group-policy/verify-install.png)

## <a name="related-articles"></a>Aiheeseen liittyviä artikkeleita

- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
- [Sovelluksen käyttöoikeuksien ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md)
- [Access-Ohjauspaneelin osa vianmääritysohjeita Internet Explorerissa](active-directory-saas-ie-troubleshooting.md)
