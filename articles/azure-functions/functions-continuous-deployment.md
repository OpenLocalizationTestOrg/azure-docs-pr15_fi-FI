<properties
   pageTitle="Jatkuva Azure-toimintojen käyttöönotto | Microsoft Azure"
   description="Jatkuva käyttöönoton tilat Azure sovelluksen-palvelun avulla voit julkaista Azure-funktiot."
   services="functions"
   documentationCenter="na"
   authors="ggailey777"
   manager="erikre"
   editor=""
   tags=""
   />

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="09/25/2016"
   ms.author="glenga"/>

# <a name="continuous-deployment-for-azure-functions"></a>Jatkuva Azure-toimintojen käyttöönotto 

Azure funktiot on helppo Jos haluat määrittää jatkuva funktio sovelluksen. Funktiot hyödyntää Azure App palvelun integrointi BitBucket, Dropbox, GitHub ja Visual Studio ryhmän Services (VSTS) käyttöön jatkuva käyttöönoton työnkulun missä Azure hakee päivitykset Funktiot-koodia, kun ne on julkaistu johonkin näistä palveluista. Jos ole ennen käyttänyt Azure-Funktiot, aloita [Azure Funktiot](functions-overview.md)yleiskuvauksessa.

Jatkuva käyttöönotto on hyvä vaihtoehto projekteille, jossa useita ja toistuvat maksut integroidaan. Se myös avulla voit säilyttää tietolähteen ohjausobjektin Funktiot-koodin. Seuraavista käyttöönoton lähteistä tällä hetkellä tueta:

+ [Bitbucket](https://bitbucket.org/)
+ [Dropbox](https://bitbucket.org/)
+ [Git paikallisen repo](../app-service-web/app-service-deploy-local-git.md)
+ Git ulkoiset repo
+ [GitHub]
+ Mercurial ulkoiset repo
+ [OneDrive](https://onedrive.live.com/)
+ Visual Studio Team Services

Ominaisuuksissa on määritetty kohden-funktion-sovelluksen välein. Jatkuva käyttöönoton on otettu käyttöön, kun funktio koodi portaalissa määritetään *vain luku-tilassa*.

## <a name="continuous-deployment-requirements"></a>Jatkuva käyttöönottovaatimukset

Määritetty käyttöönoton lähde- ja funktiot-koodi on oltava käyttöönoton lähteen ennen kuin voit aloittaa jatkuva käyttöönotto. Määritetty toiminto sovellusten käyttöönoton kukin funktio sijaitsevat nimetty alikansio, kansionimi on funktion nimi. Tämä kansiorakenne on käytettävä sivustokoodi. 

[AZURE.INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

## <a name="setting-up-continuous-deployment"></a>Jatkuva käyttöönoton määrittäminen

Jos haluat määrittää jatkuva aiemmin funktion sovelluksen noudattamalla seuraavia vaiheita:

1. Valitse funktio sovelluksen [Azure Funktiot portal](https://functions.azure.com/signin)- **funktion sovelluksen asetukset** > **määrittäminen jatkuva integrointi** > **asetukset**.

    ![Asennuksen jatkuva käyttöönotto](./media/functions-continuous-deployment/setup-deployment.png)
    
    ![Asennuksen jatkuva käyttöönotto](./media/functions-continuous-deployment/setup-deployment-1.png)
    
    Voit myös siirtyä käyttöönottoa sivu-Funktiot-pikaopas valitsemalla **Aloita tietolähteen ohjausobjektin**.

2. -Käyttöönottoa sivu, **Valitse lähteen**, valitse fill-in valitsemasi käyttöönoton lähteen tiedot ja valitse **OK**.

    ![Valitse käyttöönoton lähde](./media/functions-continuous-deployment/choose-deployment-source.png)

Kun jatkuva käyttöönoton on määritetty, kaikki muutokset tiedostot käyttöönoton lähteen kopioidaan funktion-sovellukseen ja koko sivuston käyttöönotto käynnistyy. Sivusto on myymälät päivitysajankohdan lähde-tiedostoja.


##<a name="deployment-options"></a>Käyttöönottoasetukset

Tyypillinen käyttöönoton joissakin tilanteissa ovat seuraavat:

+ 

###<a name="create-a-staging-deployment"></a>Luo väliaikaisen käyttöönotto

Funktion sovellusten vielä ei tue käyttöönoton paikkaa. Voit silti hallita eri vaiheet ja tuotannon ominaisuuksissa jatkuva integroinnin avulla.

Voit määrittää ja käyttää väliaikaisen käyttöönoton prosessin näyttää yleensä tältä:

1. Luo kaksi funktion sovelluksia tilauksen, yksi tuotannon-koodin ja yksi vaiheet. 

2. Luo käyttöönoton lähteen, jos sinulla ei ole vielä yksi. Käytämme [GitHub].
 
3. Tuotannon-funktion sovelluksen edellä mainittujen vaiheiden **määrittäminen jatkuva** käyttöönoton ja määritä käyttöönotto-haara GitHub repo perusmuodon haaran.

    ![Valitse käyttöönoton haara](./media/functions-continuous-deployment/choose-deployment-branch.png)

4. Toista tämä vaihe väliaikaisen funktion sovelluksen, mutta tällä hetkellä valita väliaikaisen haaran GitHub repo. Jos käyttöönoton lähde ei tue haarautumista, käytä toista kansiota.
 
5. Varmista päivitykset väliaikaisen haaran tai kansiosta koodi ja valitse Tarkista, että muutokset näkyvät myös väliaikaisen käyttöönotto.

6. Testauksen jälkeen Yhdistä muutokset väliaikaisesta päätasolta perusmuodon haaran kyselyjä. Tämä käynnistää käyttöönoton tuotannon funktio-sovellukseen. Jos käyttöönoton lähde ei tue haaroja, korvaa tiedostojen valmistelukansio tuotannon-kansiossa olevat tiedostot.

###<a name="move-existing-functions-to-continuous-deployment"></a>Siirtää olemassa olevat funktiot jatkuva käyttöönotto

Jos on olemassa olevat Funktiot, jotka on luodaan ja ylläpidetään portaalin, sinun täytyy ladata olemassa olevan funktion koodin tiedostojen FTP tai paikallisen Git säilöön, ennen kuin voit voit aloittaa jatkuva käyttöönoton yllä olevien ohjeiden mukaisesti. Voit tehdä tämän sovelluksen palvelun asetukset-funktio-sovelluksen. Kun tiedostot on ladattu, voit ladata ne valitsemasi jatkuva käyttöönoton lähde.

>[AZURE.NOTE]Kun määrität jatkuva integrointi, Muokkaa lähdetiedostot Funktiot-portaalissa ei enää voi.

####<a name="how-to-configure-deployment-credentials"></a>Toimintaohje: käyttöönoton tunnistetietojen määrittäminen
Ennen kuin voit ladata tiedostoja funktio-sovellukset, sinun on määritettävä tunnistetiedot voivat käyttää sivustoa, voit tehdä sen-portaalista. Tunnistetiedot on määritetty funktion sovelluksen tasolla.

1. Valitse funktio sovelluksen [Azure Funktiot portal](https://functions.azure.com/signin)- **funktion sovelluksen asetukset** > **sovelluksen palvelun asetukset** > **käyttöönoton tunnistetiedot**.

    ![Määritä paikallisen käyttöönoton tunnistetiedot](./media/functions-continuous-deployment/setup-deployment-credentials.png)

2. Kirjoita käyttäjänimi ja salasana ja valitse sitten **Tallenna**. Voit nyt käyttää funktiota sovelluksen FTP- tai valmiin Git repo näitä tunnistetietoja.

####<a name="how-to-download-files-using-ftp"></a>Toimintaohje: lataa tiedostoja FTP:

1. Valitse funktio sovelluksen [Azure Funktiot portal](https://functions.azure.com/signin)- **funktion sovelluksen asetukset** > **sovelluksen palvelun asetukset** > **Ominaisuudet** ja kopioi arvot **FTP/käyttöönoton käyttäjän**, **FTP-isäntänimi**ja **FTPS isäntänimi**.  
**FTP/käyttöönoton käyttäjän** on syötettävä sellaisina kuin ne näkyvät portaalissa, mukaan lukien sovelluksen nimen, jos haluat säätää tietää FTP-palvelimeen.

    ![Käyttöönoton tiedot](./media/functions-continuous-deployment/get-deployment-credentials.png)
    
2. FTP-asiakasohjelmassa käyttää yhteystietoja keräsit sovelluksen yhdistäminen ja ladata oman toimintojen lähdetiedostoja.

####<a name="how-to-download-files-using-the-local-git-repository"></a>Toimintaohje: lataa tiedostot paikallisen Git säilö

1. Valitse funktio sovelluksen [Azure Funktiot portal](https://functions.azure.com/signin)- **funktion sovelluksen asetukset** > **määrittäminen jatkuva integrointi** > **asetukset**.

2. -Käyttöönottoa sivu **Valitse lähde**- **paikallisen Git säilöön**, valitse sitten **OK**.
 
3. Valitse **sovellus-palvelun asetukset** > **Ominaisuudet** ja Huomautus Git URL-Osoitteen arvo. 
    
    ![Asennuksen jatkuva käyttöönotto](./media/functions-continuous-deployment/get-local-git-deployment-url.png)

4. Kloonaa repo Git tukeva komentoriviltä tai tuttuja Git-työkalun avulla paikallisesta tietokoneesta. Git Kloonaa komento näyttää seuraavalta:

        git clone https://username@my-function-app.scm.azurewebsites.net:443/my-function-app.git

5. Nouda tiedostoja funktio-sovelluksen avulla Kloonaa paikallisessa tietokoneessa, kuten seuraavassa esimerkissä:

        git pull origin master

    Pyydettäessä, Anna käyttäjänimi ja salasana, funktio-sovelluksen käyttöönottoa varten.  


[GitHub]: https://github.com/
