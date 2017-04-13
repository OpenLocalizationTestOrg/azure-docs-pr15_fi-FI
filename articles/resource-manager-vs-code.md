<properties
   pageTitle="Käytä Resurssienhallinta malleja ja koodi | Microsoft Azure"
   description="Voit määrittää Visual Studio Code Azure Resurssienhallinta-mallien luomiseen."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="cmatskas"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="chmatsk;tomfitz"/>

# <a name="working-with-azure-resource-manager-templates-in-visual-studio-code"></a>Visual Studio-koodin Azure Resurssienhallinta mallien käyttäminen

Azure Resurssienhallinta mallit ovat JSON-tiedostoja, jotka kuvaavat resurssin ja Aiheeseen liittyvät riippuvuudet. Nämä tiedostot Joskus voi olla suuri ja monimutkaiselta, joten tooling tuki on tärkeää. Visual Studio-koodi on uusi, kevyt, Avaa lähde, Office kaikissa ympäristöissä Koodieditori. Se tukee luominen ja muokkaaminen Resurssienhallinta malleja on [Uusi tunniste](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)kautta. JA koodi toimii kaikkialla ja Internet-yhteyttä ei edellytä, paitsi jos haluat ottaa Resurssienhallinta-mallit.

Jos sinulla ei ole ja koodi, voit asentaa sen osoitteessa [https://code.visualstudio.com/](https://code.visualstudio.com/).

## <a name="install-the-resource-manager-extension"></a>Asenna Resurssienhallinta-laajennus

JSON-mallien ja-koodissa käsitteleminen, sinun täytyy asentaa tiedostotunnistetta. Seuraavat vaiheet Lataa ja asenna Resurssienhallinta JSON malleja kielituki:

1. Käynnistä ja koodi 
2. Avaa nopeasti Avaa (Ctrl + P) 
3. Suorita seuraava komento: 

        ext install azurerm-vscode-tools

4. Käynnistä uudelleen ja koodin käyttöön tunnisteen pyydettäessä. 

 Työn valmis!

## <a name="set-up-resource-manager-snippets"></a>Resurssienhallinta katkelmat määrittäminen

Sillä-tuki asennetaan edelliset vaiheet, mutta nyt annettava määrittäminen ja koodi, jota käytetään JSON mallin katkelmat.

1. Kopioi tiedoston sisällön [azure-xplat-arm-sillä](https://raw.githubusercontent.com/Azure/azure-xplat-arm-tooling/master/VSCode/armsnippets.json) säilöstä Leikepöydän.
2. Käynnistä ja koodi 
3. JA-koodia, voit avata JSON katkelmat tiedoston siirtymällä joko **tiedoston** -> **asetukset** -> **Käyttäjän katkelmat** -> **JSON**tai valitsemalla **F1-näppäintä** ja kirjoittamalla **asetukset** , ennen kuin voit valita **asetukset: katkelmat**.

    ![asetus katkelmat](./media/resource-manager-vs-code/preferences-snippets.png)

    Valitse haluamasi asetukset, **JSON**.

    ![Valitse json](./media/resource-manager-vs-code/select-json.png)

4. Liitä vaiheessa 1 käyttäjä katkelmat tiedostoon lopullisessa ennen tiedoston sisällön "}" 
5. Varmista, että JSON näyttää OK ja ei ole haluat missä tahansa. 
6. Tallenna ja sulje käyttäjän katkelmat-tiedosto.

Tämä on kaikki tarvittavat Resurssienhallinta katkelmat käytön aloittamiseen. Seuraavaksi on Aseta näiden määritysten testi.

## <a name="work-with-template-in-vs-code"></a>Mallin ja koodissa käsitteleminen

Helpoin tapa aloittaa työskentelyn malli on jokin nopeasti Käynnistä mallien [Github](https://github.com/Azure/azure-quickstart-templates) tai jollakin oman. Voit helposti [viedä mallin](resource-manager-export-template.md) minkään resurssiryhmien portaalin kautta. 

1. Jos malli on viety resurssiryhmä, avaaminen ja koodin poimitun tiedostot.

    ![Näytä tiedostot](./media/resource-manager-vs-code/show-files.png)

2. Avaa template.json-tiedoston, jotta voit muokata sitä ja lisätä lisäresurssien. Sen jälkeen **"resurssien": [** painamalla enter aloittaa uuden rivin. Jos kirjoitat **arm**, esitettävä vaihtoehtojen luettelo. Nämä asetukset ovat asensit mallin katkelmat. Pitäisi näyttää tältä: 

    ![Näytä katkelmat](./media/resource-manager-vs-code/type-snippets.png)

3. Valitse haluamasi katkelmaa. Tämän artikkelin I 'M valitseminen **arm-ip** Luo uusi julkinen IP-osoite. Sijoita pilkku oikeanpuoleisen hakasulkeen jälkeen "}" Varmista, että mallin juuri luomasi resurssin syntaksi on virheellinen.

     ![Lisää pilkku](./media/resource-manager-vs-code/add-comma.png)

4. JA-koodi on valmiita IntelliSense. Voit muokata malleja, ja koodin ehdottaa käytettävissä olevat arvot. Esimerkiksi muuttujat-osan lisääminen lomakemalliin, Lisää **""** (kaksi lainausmerkkejä) ja valitse **Ctrl + VÄLINÄPPÄIN** tarjoukset välillä. Voit valita jommankumman vaihtoehdot, kuten **muuttujat**.

    ![Lisää muuttujat](./media/resource-manager-vs-code/add-variables.png)

5. IntelliSense voi myös ehdottaa käytettävissä olevat arvot tai -funktioita. Ominaisuuden määrittäminen parametrin arvon, **"[]"** -ja **Ctrl + VÄLINÄPPÄIN**lausekkeen luominen. Voit aloittaa kirjoittamisen funktion nimi. Valitse **välilehti** , kun olet löytänyt haluamasi funktio.

    ![Lisää parametri](./media/resource-manager-vs-code/select-parameters.png)

6. Valitse **Ctrl + VÄLINÄPPÄIN** uudelleen saat luettelon käytettävissä parametrien mallin sisällä funktion sisällä.

    ![Lisää parametri](./media/resource-manager-vs-code/select-avail-parameters.png)

7. Jos sinulla on kaikki mallin vahvistuksen havaitsemien virheiden mallin, näet tutut haluat editorissa. Voit tarkastella virheiden ja varoitusten luettelo kirjoittamalla **Ctrl + VAIHTO + M** tai kuvioita valitsemalla näytön vasemmassa tilarivillä.

    ![virheet](./media/resource-manager-vs-code/errors.png)

    Vahvistus mallin avulla voit tunnistaa syntaksi ongelmiin. Voit kuitenkin myös nähdä virheitä, joita voit jättää huomiotta. Joissakin tapauksissa editorin vertaa mallin mallin pohjalta, joka ei ole ajan tasalla ja raportoi virheen vuoksi, vaikka tiedät, että se on oikein. Oletetaan, että funktion on äskettäin lisätty Resurssienhallinta, mutta rakennetta ei ole päivitetty. Editorin ilmoittaa virheestä huolimatta siitä, funktio toimii oikein käyttöönoton aikana.

    ![virhesanoma](./media/resource-manager-vs-code/unrecognized-function.png)

## <a name="deploy-your-new-resources"></a>Ottaa käyttöön uudet resurssit

Kun malli on valmis, voit ottaa käyttöön uudet resurssit seuraavia ohjeita: 

### <a name="windows"></a>Windows

1. Avaa PowerShellin komentorivi-ikkuna 
2. Kirjaudu sisään tyyppi: 

        Login-AzureRmAccount 

3. Jos sinulla on useita tilauksia, saat luettelon tilauksista kanssa:

        Get-AzureRmSubscription

    Ja käyttämään tilaus.
   
        Select-AzureRmSubscription -SubscriptionId <Subscription Id>

4. Päivitä parameters.json tiedostossa parametrit
5. Voit ottaa mallin käyttöön Azure Deploy.ps1 suorittaminen

### <a name="osxlinux"></a>OS x tai Linux

1. Avaa pääte-ikkuna 
2. Kirjaudu sisään tyyppi:

        azure login 

3. Jos sinulla on useita tilauksia, valitse oikeanpuoleisessa tilaus:

        azure account set <subscriptionNameOrId> 

4. Päivitä parameters.json tiedoston parametrit.
5. Jos haluat ottaa mallin, suorita:

        azure group deployment create -f <PathToTemplate> 

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja mallit-kohdassa [yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja](resource-group-authoring-templates.md).
- Lisätietoja mallin funktiot on artikkelissa [Azure Resurssienhallinta malli-Funktiot](resource-group-template-functions.md).
- Visual Studio Code käsitteleminen Lisää esimerkkejä on kohdassa [Muodosta cloud Visual Studio koodia sisältävien](https://github.com/Microsoft/HealthClinic.biz/wiki/Build-cloud-apps-with-Visual-Studio-Code) - [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Yhdistä [esittely](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Katso Lisää quickstarts-HealthClinic.biz esittely, [Azure Developer Työkalut Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).
