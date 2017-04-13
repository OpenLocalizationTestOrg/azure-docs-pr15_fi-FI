<properties
    pageTitle="Mitkä ovat Azure MFA-sovelluksen salasanat?"
    description="Tällä sivulla auttaa käyttäjiä ymmärtämään Azure MFA huomioon sovelluksen salasanat ovat ja miten niitä käytetään kanssa."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>



# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a>Mitkä ovat sovelluksen salasanat-Azure Monimenetelmäisen todentamisen?

Tiettyjen selaimen sovellusten, kuten Apple alkuperäisen sähköpostin asiakas, joka käyttää Exchange Active Sync tällä hetkellä tue monimenetelmäisen todentamisen. Monimenetelmäisen todentamisen on käytössä käyttäjää kohden. Tämä tarkoittaa, että jos käyttäjä on otettu käyttöön monimenetelmäisen todentamisen ja niiden yrität käyttää selaimen sovelluksia, ne näkyvät epäonnistui. Sovelluksen salasanan avulla tätä.

>[AZURE.NOTE] Nykyaikainen todentaminen Office 2013-asiakasohjelmat
>
> Office 2013: n asiakkaiden (mukaan lukien Outlook) tue uusi todennusprotokollia ja on määritetty tukemaan Monimenetelmäisen todentamisen nyt.  Tämä tarkoittaa, että kun otettu käyttöön, sovelluksen salasanat eivät ole pakollisia käytettäväksi Office 2013-asiakkaiden kanssa.  Katso lisätietoja [Office 2013: n Nykyaikainen todentaminen julkisen esikatselu ilmoitettiin](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

## <a name="how-to-use-app-passwords"></a>Voit käyttää sovelluksen salasanat

Seuraavat asiat muistaa käyttämisestä sovelluksen salasanat.

- Todellinen salasana luodaan automaattisesti ja käyttäjä ei ole toimitettu. Tämä johtuu siitä automaattisesti luotu salasana on näyttävyyttä hyödyntämällä arvata ja on entistä turvallisempi.
- Tällä hetkellä on 40 salasanojen käyttäjäkohtainen rajoitukset. Jos yrität luoda, kun olet saavuttanut rajoitus-voit pyydetään poistettava jonkin olemassa olevan ohjelmasalasanat jotta voit luoda uuden.
- On suositeltavaa, että sovelluksen salasanat luodaan laitteen ja ei sovelluksen kohti. Voit esimerkiksi luoda yhden ohjelman salasana kannettavaan ja käyttää kaikkien sovellustesi, kannettavalla kyseisen ohjelman salasana.
- On annettu sovelluksen salasanan kirjaudut sisään ensimmäisen kerran.  Jos tarvitset lisää kommentteja, voit luoda niitä.

![Asetukset](./media/multi-factor-authentication-end-user-app-passwords/app.png)

Kun ohjelman salasanaa, käytät tätä sijasta alkuperäinen salasana näiden selaimen sovellusten kanssa.  Siten esimerkiksi, jos käytössäsi on monimenetelmäisen todentamisen ja Apple alkuperäisen sähköpostiohjelmat puhelimessa.  Käytä ohjelman salasana, niin, että voit ohittaa monimenetelmäisen todentamisen ja jatkaa.

## <a name="creating-and-deleting-app-passwords"></a>Luominen ja poistaminen sovelluksen salasanat
Oman alkuperäinen-kirjautuminen aikana annetaan sovelluksen salasanan, jonka avulla.  Lisäksi voit luoda ja poistaa sovelluksen salasanat myöhemmin.  Miten teet tämän määräytyy sen mukaan, miten voit käyttää monimenetelmäisen todentamisen.  Valitse se, että suurin sinulle.

Multi-factor authentiation käyttämisestä|Kuvaus
:------------- | :------------- |
[Käytän Office 365: ssä](#creating-and-deleting-app-passwords-with-office-365)|  Tämä tarkoittaa, että haluat luoda sovelluksen salasanat – Office 365-portaalissa.
[En tiedä](#creating-and-deleting-app-passwords-with-myapps-portal)|Tämä tarkoittaa sitä, sinun kannattaa luoda ohjelmasalasanat [https://myapps.microsoft.com](https://myapps.microsoft.com) kautta
[Sitä käytetään Microsoft Azure kanssa](#create-app-passwords-in-the-azure-portal)| Tämä tarkoittaa, että haluat luoda sovelluksen salasanat palvelun Azure-portaalissa.

## <a name="creating-and-deleting-app-passwords-with-office-365"></a>Luominen ja poistaminen Office 365: n sovelluksen salasanat

Jos käytät Office 365: n sovellushakemistosivusto kannattaa luoda ja poistaa sovelluksen salasanat – Office 365-portaalin monimenetelmäisen todentamisen.

### <a name="to-create-app-passwords-in-the-office-365-portal"></a>Sovelluksen salasanat luominen Office 365-portaalissa
--------------------------------------------------------------------------------

1. Kirjaudu [Office 365-portaalissa](https://login.microsoftonline.com/).
2. Ikkunan oikeassa yläkulmassa widget ja valitse Office 365-asetukset.
3. Valitse lisäsuojauksen vahvistus.
4. Oikeanpuoleiseen napsauttamalla linkkiä, jossa lukee **päivittää Omat puhelinnumerot tilin suojaukseen.** 
 ![Asetukset](./media/multi-factor-authentication-end-user-manage/o365a.png)
5. Tällöin näyttöön sivu, jotta voit muuttaa asetuksia.
![Cloud](./media/multi-factor-authentication-end-user-manage/o365b.png)
6. Ylhäällä vieressä lisäsuojauksen tarkistus, valitse **ohjelmasalasanat.**
7. Valitse **Luo**.
![Cloud](./media/multi-factor-authentication-end-user-app-passwords-create-o365/apppass.png)
8. Kirjoita nimi sovelluksen salasana ja valitse **Seuraava**.
![Sovelluksen salasanan luominen](./media/multi-factor-authentication-end-user-app-passwords/create1.png)
9. Ohjelman salasana Kopioi Leikepöydälle ja liitä sovelluksen.
![Sovelluksen salasanan luominen](./media/multi-factor-authentication-end-user-app-passwords/create2.png)


### <a name="to-delete-app-passwords-using-the-office-365-portal"></a>Voit poistaa sovelluksen salasanat Office 365-portaalissa
--------------------------------------------------------------------------------


1. Kirjaudu [Office 365-portaalissa](https://login.microsoftonline.com/).
2. Ikkunan oikeassa yläkulmassa widget ja valitse Office 365-asetukset.
3. Valitse lisäsuojauksen vahvistus.
4. Oikeanpuoleiseen napsauttamalla linkkiä, jossa lukee **päivittää Omat puhelinnumerot tilin suojaukseen.** 
 ![Asetukset](./media/multi-factor-authentication-end-user-manage/o365a.png)
5. Tällöin näyttöön sivu, jotta voit muuttaa asetuksia.
![Cloud](./media/multi-factor-authentication-end-user-manage/o365b.png)
6. Ylhäällä vieressä lisäsuojauksen tarkistus, valitse **ohjelmasalasanat.**
7. Valitse poistettavien ohjelman salasana, vieressä **Poista**.
![Sovelluksen salasanan poistaminen](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)
8. Vahvista poisto valitsemalla **Kyllä**.
![Vahvista](./media/multi-factor-authentication-end-user-app-passwords/delete2.png)
9. Kun ohjelman salasana poistetaan valitsemalla **Sulje**.
![Sulje](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)


## <a name="creating-and-deleting-app-passwords-with-myapps-portal"></a>Luominen ja poistaminen sovelluksen salasanat Omat sovellukset-portaalissa.
Jos et ole varma, kuinka voit käyttää monimenetelmäisen todentamisen, valitse voit aina luoda ja poistaa sovelluksen salasanat palvelun Omat sovellukset-portaalissa.

### <a name="to-create-an-app-password-using-the-myapps-portal"></a>Voit luoda omat sovellukset-portaalissa sovelluksen salasanan

1. Kirjautuminen [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Ylhäällä Valitse profiili.
3. Valitse lisäsuojauksen vahvistus.
![Cloud](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. Tällöin näyttöön sivu, jotta voit muuttaa asetuksia.
![Asetukset](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)
5. Ylhäällä vieressä lisäsuojauksen tarkistus, valitse **ohjelmasalasanat.**
6. Valitse **Luo**.
![Sovelluksen salasanan luominen](./media/multi-factor-authentication-end-user-app-passwords/create3.png)
7. Kirjoita nimi sovelluksen salasana ja valitse **Seuraava**.
![Sovelluksen salasanan luominen](./media/multi-factor-authentication-end-user-app-passwords/create1.png)
8. Ohjelman salasana Kopioi Leikepöydälle ja liitä sovelluksen.
![Sovelluksen salasanan luominen](./media/multi-factor-authentication-end-user-app-passwords/create2.png)

### <a name="to-delete-an-app-password-using-the-myapps-portal"></a>Voit poistaa sovelluksen salasanan Omat sovellukset-portaalissa

1. Kirjautuminen [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Ylhäällä Valitse profiili.
3. Valitse lisäsuojauksen vahvistus.
![Cloud](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. Tällöin näyttöön sivu, jotta voit muuttaa asetuksia.
![Asetukset](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)
5. Ylhäällä vieressä lisäsuojauksen tarkistus, valitse **ohjelmasalasanat.**
6. Valitse poistettavien ohjelman salasana, vieressä **Poista**.
![Sovelluksen salasanan poistaminen](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)
7. Vahvista poisto valitsemalla **Kyllä**.
![Vahvista](./media/multi-factor-authentication-end-user-app-passwords/delete2.png)
8. Kun ohjelman salasana poistetaan valitsemalla **Sulje**.
![Sulje](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)


## <a name="create-app-passwords-in-the-azure-portal"></a>Luo sovelluksen salasanat Azure-portaalissa

Jos haluat luoda sovelluksen salasanat palvelun Azure-portaalissa Azure monimenetelmäisen todentamisen.

### <a name="to-create-app-passwords-in-the-azure-portal"></a>Luo sovelluksen salasanat Azure-portaalissa

1. Kirjaudu sisään Azure hallinta-portaaliin.
2. Ylhäällä Napsauta käyttäjänimeä hiiren kakkospainikkeella ja valitse Lisää suojauksen vahvistus.
3. Proofup-sivun yläreunassa, valitse sovelluksen salasanat
4. Valitse **Luo**.
5. Kirjoita nimi sovelluksen salasana ja valitse **Seuraava**
6. Ohjelman salasana Kopioi Leikepöydälle ja liitä sovelluksen.
![Cloud](./media/multi-factor-authentication-end-user-app-passwords-create-azure/app2.png)

### <a name="to-delete-app-passwords-in-the-azure-portal"></a>Voit poistaa sovelluksen salasanat Azure-portaalissa

1. Kirjaudu sisään Azure hallinta-portaaliin.
2. Ylhäällä Napsauta käyttäjänimeä hiiren kakkospainikkeella ja valitse Lisää suojauksen vahvistus.
3. Ylhäällä vieressä lisäsuojauksen tarkistus, valitse **ohjelmasalasanat.**
4. Valitse poistettavien ohjelman salasana, vieressä **Poista**.
5. Vahvista poisto valitsemalla **Kyllä**.
6. Kun ohjelman salasana poistetaan valitsemalla **Sulje**.
![Sulje](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)
