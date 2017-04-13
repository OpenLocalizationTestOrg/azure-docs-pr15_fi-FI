<properties
    pageTitle="Luo palautus suunnitelmien | Microsoft Azure" 
    description="Luo palautus suunnitelmien Azure palauttaminen epäonnistuu päälle ja palauttaa näennäiskoneiden ja fyysinen palvelimiin." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="create-recovery-plans"></a>Luo palautus-Palvelupaketit

Azure palauttaminen-palvelun osaltaan mukaan orchestrating replikoinnin, automaattisesti ja näennäiskoneiden ja fyysiset palvelimet yrityksen liiketoiminnan jatkuvuuden ja tietojen palauttaminen (BCDR) määrittäminen. Koneet voi replikoida Azure tai toissijaisen paikallisen tietokeskuksen. Lue pikaisesti [Azure palauttaminen ominaisuudet?](site-recovery-overview.md).


## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa on tietoja luomisesta ja mukauttamisesta palautus-palvelupakettia. 

Palautus suunnitelmien koostuvat järjestetyn ryhmiä, jotka sisältävät näennäiskoneiden tai fyysinen palvelimissa, jotka haluat epäonnistua yhdessä päälle. Palautus suunnitelmien seuraavasti:

- Määritä koneet, epäonnistua päälle ja Käynnistä yhdessä ryhmät.
- Mallin riippuvuudet ryhmittelemällä ne yhteen palautus suunnitelman ryhmän laitteiden välillä. Esimerkiksi jos epäonnistua päälle ja tuo näkyviin tietyn sovelluksen ryhmittelee näennäiskoneiden sovelluksen samassa palautus suunnittelu-ryhmässä.
- Automatisoida ja laajentaa automaattisesti. Voit suorittaa testin suunniteltu tai suunnittelematon automaattisesti palautus-palvelupaketti. Voit mukauttaa palautus suunnitelmien komentosarjoja, Azure Automaattiset ja manuaaliset toiminnot.

Palautus suunnitelmien näkyvät **Palautus suunnitelmien** sivuston palautus-portaalissa.


Kirjaa kommentteja tai kysymyksiä alaosassa on tämän artikkelin tai [Azure palautus Services keskustelupalstalle](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Ennen aloittamista

Ota seuraavat seikat huomioon:

- Palautus-suunnitelma ei pidä sekoittaa VMs yksittäisten ja useiden verkkosovittimien. Tämä johtuu siitä sekoittamalla nämä VMs ei ole sallittua Azure pilvipalvelussa.
- Voit laajentaa palautus suunnitelmien komentosarjojen ja manuaalinen toiminnot. Ota seuraavat seikat huomioon:
    - Kirjoita komentosarjoja Windows PowerShellin avulla.
    - Varmistaa, että komentosarjojen käyttää yritä todellisen lohkot niin, että poikkeukset käsitellään tilanteen. Jos poikkeus komentosarjan sen suoritus keskeytyy ja tehtävän näyttää epäonnistui.  Jos virhe ilmenee, komentosarjan loppuosa ei suoriteta. Jos tämä tapahtuu, kun käytät suunnittelematon automaattisesti, palautus-suunnitelma säilyvät. Jos näin tapahtuu, kun käytössäsi on suunniteltu automaattisesti, ne eivät enää palautus-suunnitelma. Jos näin tapahtuu, komentosarjan korjaaminen, varmista, että se toimii halutulla tavalla ja suorita sitten palautus-suunnitelma uudelleen.
    - Kirjoita Host-komento ei toimi palautus suunnitelman komentosarjan ja komentosarja epäonnistuu. Jos haluat luoda tulostus-välityspalvelimen komentosarjan, joka suorittaa puolestaan tärkeimmät komentosarjan luominen ja varmistaa, että kaikki tulosteen piped, käytössä >> komento.
    - Komentosarjan aikakatkaistaan jos se ei palauta sisällä 600 sekuntia.
    - Jos mitään kirjoitetaan STDERR-komentosarja voidaan luokitella, kuten epäonnistui. Nämä tiedot näkyvät komentosarjojen suorittamisen lisätiedot.
    - Jos käytät VMM käyttöönoton Huomaa, että:

        - Palautus-suunnitelmassa komentosarjat suoritetaan VMM palvelutilin kontekstissa. Varmista, että tili on lukuoikeudet remote verkkosijainnissa, jossa komentosarja on, ja testaa komentosarja suoritetaan VMM palvelun tili-käyttöoikeustaso.
        - Windows PowerShell-moduulin toimitetaan VMM cmdlet-komennot. VMM Windows PowerShell-moduuli on asennettu VMM-konsolin asennuksen yhteydessä. VMM-moduuli on ladattu komentosarjan käyttäminen komentosarja seuraava komento: Tuo moduuli-nimen virtualmachinemanager. [Lisätietoja](hhttps://technet.microsoft.com/library/hh875013.aspx).
        - Varmista VMM käyttöönoton on vähintään yksi kirjasto-palvelin. Oletusarvoisesti kirjaston Jaa-polku, VMM-palvelin sijaitsee paikallisesti kansionimi MSCVMMLibrary VMM-palvelimella.
        - Jos kirjaston Jaa-polku on remote (tai paikalliseen mutta ei jaeta MSCVMMLibrary kanssa, Määritä Jaa seuraavasti (käyttämällä \\libserver2.contoso.com\share\ esimerkkinä):
            - Avaa Rekisterieditori ja siirry HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure sivuston Recovery\Registration.
            -  Muokkaa arvoa ScriptLibraryPath ja sijoita arvon kuin \\libserver2.contoso.com\share\. määrittää koko FQDN. Anna jaetun kansion käyttöoikeudet.
            -  Varmista testata komentosarja käyttäjätili, jolla on samat oikeudet kuin VMM-palvelutilin varmistaa, että erillinen testattu komentosarjat suoritetaan samalla tavalla, että ne tulevat palautus-palvelupakettien kanssa. VMM palvelimessa Määritä suorittamisen käytäntö ohittaa seuraavasti:
                -  Avaa 64-bittinen Windows PowerShell console käyttämällä laajentamiseen.
                -  Tyyppi: **Määritä suorituskäytäntöä ohittaa**. [Lisätietoja](https://technet.microsoft.com/library/ee176961.aspx).

## <a name="create-a-recovery-plan"></a>Suunnittele palauttaminen

Tapa, jossa luot palautus suunnitelman määräytyy sivuston palauttamisen käyttöönotto.

- **Replikoiminen VMware VMs ja fyysinen palvelimissa**, Suunnittele ja lisätä replikoinnin ryhmiä, jotka sisältävät näennäiskoneiden ja fyysiset palvelimet palautus-palvelupakettia.
- **Replikoiminen Hyper-V VMs (Valitse VMM paveikslėlis)**– luo suunnitelma ja lisätä suojatun Hyper-V näennäiskoneiden VMM pilvestä palautus suunnitelma.
- **Replikoiminen Hyper-V VMs (eivät sisälly VMM paveikslėlis)**— Suunnittele ja suojatun Hyper-V näennäiskoneiden lisääminen palautus suunnitelma suojaus-ryhmä.
- **SAN replikoinnin**– suunnitelman luominen ja lisääminen replikoinnin-ryhmä, joka sisältää näennäiskoneiden palautus-palvelupakettia. Voit valita replikoinnin ryhmän tietyn näennäiskoneiden sijaan, koska kaikki replikoinnin ryhmän näennäiskoneiden on epäonnistua päälle yhdessä (vikasietotila ilmenee tallennustilan tasolla ensin).


Suunnittele palautus seuraavasti:

1. **Suunnitelmien palautus** -välilehti > **Luo palautus suunnitteleminen**.
Määritä palautus-palvelupakettia, ja lähde- ja kohdesivustojen nimi. Lähdepalvelimeen on oltava näennäiskoneiden, joissa on otettu käyttöön automaattisesti ja palauttaminen.

    - Jos olet replikoiminen VMM-VMM, valitse **Lähdetyyppi** > **VMM**ja lähde- ja VMM-palvelimiin. Napsauttamalla saat näkyviin paveikslėlis, joka on määritetty käyttämään Hyper-V replikan **Hyper-V** . 
    - Jos olet replikoiminen VMM-, SAN Valitse **Lähdetyyppi**käyttäen VMM > **VMM**ja lähde- ja VMM-palvelimiin. Valitse **SAN** paveikslėlis, jotka on määritetty SAN replikoinnin näkevän.
    - Jos olet replikoiminen from VMM Azure, valitse **Lähdetyyppi** > **VMM**.  Valitse VMM lähdepalvelin ja **Azure** kohteena.
    - Jos olet replikoiminen Hyper-V-sivustosta valitsemalla **Lähdetyyppi** > **Hyper-V-sivustossa**. Valitse sivuston lähde- ja **Azure **kohteena.
    - Jos olet replikoiminen VMware tai fyysinen paikallisen palvelimelta Azure, valitse configuration-palvelin lähde- ja **Azure** kohteeksi

2. Valitse **Valitse näennäiskoneiden** , johon haluat lisätä palautus-suunnitelmassa oletusryhmän (ryhmä 1) näennäiskoneiden (tai replikoinnin ryhmä).

## <a name="customize-recovery-plans"></a>Mukauta palautus-Palvelupaketit

Kun olet lisännyt suojattu näennäiskoneiden tai replikoinnin ryhmien palautus suunnitelman oletusryhmään ja luoda palvelupaketti, voit mukauttaa sen:

- **Lisää uusia ryhmiä**, voit lisätä muita palautus suunnitteluryhmät. Voit lisätä ryhmät numeroidaan siinä järjestyksessä, jossa voit lisätä ne. Voit lisätä enintään seitsemän ryhmät. Voit lisätä useita koneet tai replikoinnin ryhmien uusi ryhmiin. Huomaa, että virtuaalikoneen tai replikoinnin ryhmän vain voidaan lisätä yhden palautus suunnitelman ryhmän.
- **Lisää komentosarja **, voit lisätä komentosarjoja tähän tarkoitukseen ennen tai jälkeen palautuksen suunnitteleminen ryhmän. Komentosarjan lisääminen Lisää uusi joukko toimintoja ryhmän. Esimerkiksi vanhat vaiheita ryhmän 1 luodaan nimellä: ryhmän 1: vanhat vaiheet. Vanhat vaiheiden suorittamisesta näkyvät tämän kohdan arvoksi sisällä. Huomaa, että niitä voi lisätä ainoastaan komentosarjan ensisijainen sivustossa Jos sinulla on otettu käyttöön VMM palvelimeen.
- **Manuaalinen toiminnon lisääminen**— voit lisätä manuaalisen toimintoja, jotka suoritetaan ennen tai jälkeen palautus suunnitelman ryhmän. Kun palautus-suunnitelma suoritetaan, pysähtyy kohtaan, johon lisäsit manuaalisia toimia ja valintaikkunan kehottaa sinua Manuaalinen toiminto suoritettiin.

## <a name="extend-recovery-plans-with-scripts"></a>Laajenna palautus suunnitelmien komentosarjojen kanssa

Voit lisätä komentosarjan palautus-palvelupaketti:

- Jos sinulla on VMM lähdesivuston, voit luoda komentosarjan VMM palvelimessa ja ovat palauttamista suunnitelmassa.
- Jos olet replikoiminen Azure avulla voit integroida Azure automaatio runbooks palautus-suunnitelma

### <a name="create-a-vmm-script"></a>VMM komentosarjan luominen


Luo komentosarja seuraavasti:

1. Luo uusi kansio esimerkiksi kirjaston Jaa- \<VMMServerName > \MSSCVMMLibrary\RPScripts. Sijoittaa lähde ja kohde VMM palvelimiin.
2. Luoda komentosarjan (esimerkiksi RPScript) ja valitse se toimii odotetulla tavalla.
3. Siirrä komentosarja sijainti \<VMMServerName > \MSSCVMMLibrary lähde- ja VMM-palvelimiin.

### <a name="create-an-azure-automation-runbook"></a>Luo Azure automaatio-runbookin

Voit laajentaa palautus palvelupaketin suorittamalla Azure automaatio-runbookin osana suunnitelma. [Lue lisää](site-recovery-runbook-automation.md).


### <a name="add-custom-settings-to-a-recovery-plan"></a>Lisätä mukautettuja asetuksia palautus-suunnitelma

1. Avaa mukautettavan palautus suunnitelma.
2. Lisää näennäiskoneiden tai uusi ryhmä.
3. Lisättävä komentosarja tai Manuaalinen toiminnon **Vaihe** luettelossa olevan kohteen napsauttamalla ja valitse sitten **komentosarja** tai **Manuaalinen toiminto**. Määrittää, haluatko lisätä komentosarjan tai ennen tai jälkeen valitun kohteen toiminnon. **Siirrä ylös** - ja **Siirrä alas** -komentopainikkeet avulla voit komentosarja Siirrä ylös tai alas.
4. Jos lisäät VMM komentosarjan, valitse **VMM komentosarjan automaattisesti**ja - **Komentosarjapolku** Kirjoita Jaa suhteellisen polun. Näin on, missä Jaa osoitteessa esimerkissä \\ <VMMServerName>\MSSCVMMLibrary\RPScripts, Määritä polku: \RPScripts\RPScript.PS1.
5. Jos olet lisäämässä Azure automaatio Suorita Bookin, Määritä **Azure automaatio-tiliin** , jossa: n runbookin sijaitsee, ja valitse haluamasi **Azure Runbookin komentosarjan**.
5. Tee automaattisesti palautus-palvelupaketin voit varmistaa, että komentosarjan toimii odotetulla tavalla.


## <a name="next-steps"></a>Seuraavat vaiheet

Voit suorittaa erilaisia failovers palautus-palvelupaketti, mukaan lukien testi automaattisesti, voit tarkistaa ympäristön ja suunniteltuja tai suunnittelemattomia failovers. [Lue lisää](site-recovery-failover.md).


 
