<properties
    pageTitle="Käytön aloittaminen tallennustilan Explorer (ennakkoversio) | Microsoft Azure"
    description="Azure-tallennustilan resurssien tallennustilan Resurssienhallinnassa (ennakkoversio)"
    services="storage"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />

 <tags
    ms.service="storage"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/17/2016"
    ms.author="tarcher" />

# <a name="getting-started-with-storage-explorer-preview"></a>Käytön aloittaminen tallennustilan Explorer (ennakkoversio)

## <a name="overview"></a>Yleiskatsaus 

Microsoft Azure tallennustilan Explorer (ennakkoversio) on erillinen sovellus, jonka avulla voit helposti käyttää Azuren tallennustilaan tiedoilla Windows, OS X- ja Linux. Tässä artikkelissa käydään eri tapoja muodostaa ja Azure tallennustilan asiakkaiden hallinta.

![Microsoft Azure-tallennustilan Explorer (ennakkoversio)][15]

## <a name="prerequisites"></a>Edellytykset

- [Lataa ja asenna tallennustilan Explorer (ennakkoversio)](http://www.storageexplorer.com)

## <a name="connect-to-a-storage-account-or-service"></a>Yhteyden muodostaminen tallennustilan tili tai palvelu

Tallennustilan Explorer (ennakkoversio) on myriad tapoja tallennustilan tileihin yhdistäminen. Tämä vaihtoehto sisältää yhteyden muodostamisesta tallennustilan tileihin liittyvän Azure tilaukset-yhteyden muodostamisesta tallennustilan asiakkaat ja palvelut, jotka on jaettu muut Azure-tilaukset ja jopa yhteyden ja paikallisen tallennustilan käyttämällä emulaattorin Azure-tallennustilan hallinta:

- [Yhteyden muodostaminen Azure tilauksen](#connect-to-an-azure-subscription) - tallennustilan resurssien kuuluvat Azure-tilaukseen.
- [Paikallista kehittämistä tallennustilan käsitteleminen](#work-with-local-development-storage) - käyttämällä Azure-tallennustilan emulaattorin paikallisen tallennustilan hallinta. 
- [Liitä ulkoisille](#attach-or-detach-an-external-storage-account) - tallennustilan resurssien kuuluvat toiseen Azure tilauksen tallennustilan tilin nimi ja avaimen avulla.
- [Liitä-tallennustilan tilin käyttämällä SAS](#attach-storage-account-using-sas) - tallennustilan resurssien toiseen Azure tilaukseesi SAS kuuluvat.
- [Liitä-palvelun avulla SAS](#attach-service-using-sas) - hallita tietyn tallennuspalvelu (blob-säilö, jonossa tai taulukko) toisen Azure tilaukseesi SAS kuuluvat.

## <a name="connect-to-an-azure-subscription"></a>Yhteyden muodostaminen Azure tilauksen

> [AZURE.NOTE] Jos sinulla ei ole Azure-tili, voit [Rekisteröidy maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) tai [aktivoida Visual Studio tilaajan etuja](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

1. Valitse Explorerissa Storage (ennakkoversio) **Azure tilin asetukset**. 

    ![Azure tilin asetukset][0]

1. Vasemmassa ruudussa näkyy nyt kaikki olet kirjautunut Microsoft-tilit. Yhteyden muodostaminen toiseen tiliin, valitse **Lisää tili**ja seuraa valintaikkunat kirjautumaan sisään Microsoft-tili, joka on liitetty vähintään yksi aktiivinen Azure-tilaus.

    ![Tilin lisääminen][1]

1. Kun kirjaudut sisään Microsoft-tilillä onnistuneesti, vasemmanpuoleisessa ruudussa lisätä tiliin liittyvien Azure tilaukset. Valitse Azure tilaukset, jossa haluat käyttää, ja valitse sitten **Käytä**. (Valitsemalla **kaikki tilaukset** näyttää tai piilottaa valitsemalla kaikki tai Älä yhtäkään luettelossa Azure tilaukset).

    ![Valitse Azure-tilaukset][3]

1. Vasemmanpuoleisessa ruudussa tuo näyttöön valitun Azure tilaukset liittyvät tallennustilan tilit.

    ![Valitun Azure tilaukset][4]

## <a name="work-with-local-development-storage"></a>Paikallista kehittämistä tallennustilan käsitteleminen

Tallennustilan Explorer (ennakkoversio) avulla voit tallentaa paikallisesti Azure-tallennustilan emulaattorin vastaan toimi. Näin voit vastaan koodin kirjoittaminen ja testaa tallennustilan ilman varsinaista tallennustilan-tilin käyttöön Azure (koska tallennustilan-tili on käytössä emuloida Azure-tallennustilan emulaattorin).

>[AZURE.NOTE] Azure-tallennustilan emulaattorin tukee tällä hetkellä vain Windows. 

1. Laajenna vasemmanpuoleisessa ruudussa tallennustilan Explorer (esikatselu) **(paikallinen ja liitetty** > **Tallennustilan tilit** > solmu**(kehitys)** .

    ![Paikallista kehittämistä solmu][21]

1. Jos et ole vielä asentanut Azure-tallennustilan emulaattorin, sinua pyydetään tekemään niin tietopalkki kautta. Jos näyttöön tulee tietopalkissa, **Lataa uusin versio**ja asenna emulaattori. 

    ![Lataa Azure-tallennustilan emulaattorin kehote][22]

1. Kun emulaattori on asennettu, sinun on mahdollisuus luominen ja käyttäminen paikallisen BLOB-objektit ja olevien taulukoiden. Lisätietoja kunkin tyyppiselle tilille tallennustilan käyttöä varten, valitse haluamaasi linkkiä:

    - [Azure-blob storage resurssien hallinta](./vs-azure-tools-storage-explorer-blobs.md)
    - Resurssien Azure tiedoston Jaa tallennustilan – *tulossa pian*
    - Resurssien Azure jonon tallennustilan – *tulossa pian*
    - Azuren taulukkojen tallennustilaan resurssit - *tulossa* hallinta

## <a name="attach-or-detach-an-external-storage-account"></a>Liitä tai irrota ulkoisille tili

Tallennustilan Explorer (ennakkoversio) mahdollistaa liittäminen ulkoisille tilit niin, että tallennustilan tilejä voidaan jakaa helposti. Tässä osassa kerrotaan, miten voit liittää (ja irrota) ulkoisille tilit.

### <a name="get-the-storage-account-credentials"></a>Hae tallennustilan tilin tunnistetiedot

Jakaa ulkoisille tili, jotta kyseisen tilin omistajan on ensin gatewaylta - tilin nimi ja avain - tilin ja jaa tietojen liittäminen (ulkoinen) tilin kenties henkilön kanssa. Tallennustilan tilin tunnistetiedot hankkiminen voidaan tehdä Azure portaalin kautta toimimalla seuraavasti: 

1.  Kirjautuminen [Azure portal](https://portal.azure.com).
1.  Valitse **Selaa**.
1.  Valitse **tallennustilan tilit**.
1.  Valitse haluamasi tallennustilan tilin **Tallennustilan tilit** -sivu.
1.  Valitse valitun tallennustilan tilin **asetukset** -sivu **pikanäppäimet**.

    ![Käyttöasetus näppäimet][5]
    
1.  Kopioi **valintanäppäinten** -sivu **TALLENNUSTILAN tilin nimi** ja **avain 1** arvot käytettäväksi pienenä tallennustilan-tilille. 

    ![Pikanäppäimet][6]

### <a name="attach-to-an-external-storage-account"></a>Liitä ulkoisille-tiliin
Voit liittää ulkoisille tilin, sinun on asiakkaan nimi ja avaimen avulla. *Tallennustilan tilin Gatewaylta* jaksossa kerrotaan, miten voit hankkia nämä arvot Azure-portaalista. Huomaa kuitenkin, että portaalissa, tili-näppäintä, jonka nimi on "avaimen 1" niin tallennustilan Explorer (ennakkoversio) pyytää tili-näppäintä, jos kirjoittamasi (tai liittämäsi) "-avain 1"-arvo. 
 
1.  Valitse tallennustilan Explorerissa (ennakkoversio), **Muodosta yhteys Azure-tallennustilan**.

    ![Azure tallennuspaikka ja yhdistäminen][23]

1.  Valitse **Muodosta yhteys Azuren tallennustilaan** -valintaikkunassa Määritä tili key (Azure-portaalista "avaimen 1" arvo) ja valitse sitten **Seuraava**.

    ![Yhteyden muodostaminen Azure tallennustilan-valintaikkuna][24] 

1.  **Liittää ulkoisen tallennustilan** -valintaikkunassa tallennustilan tilin nimi Kirjoita **tilin nimi** -ruutuun, Määritä muut haluamasi asetukset ja valitse **Seuraava** kierron. 

    ![Liitä ulkoisille-valintaikkuna][8]

1.  **Yhteyden yhteenveto** -valintaikkunassa Tarkista tiedot. Jos haluat vaihtaa, valitse **takaisin** ja kirjoita haluamasi asetukset uudelleen. Kun olet valmis, valitse **Yhdistä**.

1.  Kun yhteys on muodostettu, ulkoisille tilin näkyy teksti **(ulkoinen)** liitetään tallennustilan tilin nimi. 

    ![Yhteyden muodostaminen ulkoisille tilin tulos][9]

### <a name="detach-from-an-external-storage-account"></a>Irrota ulkoisille-tilistä

1.  Haluat irrottaa ulkoisille tilin hiiren kakkospainikkeella ja - pikavalikko - kohdassa **Katkaise yhteys**.

    ![Irrota tallennuspaikka][10]

1.  Kun vahvistus-sanomaruutu tulee näkyviin, valitse **Kyllä** Vahvista aiottuun ulkoisille-tililtä.

## <a name="attach-storage-account-using-sas"></a>Liitä tallennustilan tilillä Suojaussidokset

[SAS (jaettu Access allekirjoitus)](storage/storage-dotnet-shared-access-signature-part-1.md) antaa Azure tilauksen voi myöntää käyttöoikeuksia tallennustilan tilin väliaikaisesti kirjoittamatta Azure tilauksen käyttöoikeuksien hallinta. 

Esitä tämä oletetaan UserA on Azure-tilauksen järjestelmänvalvoja ja UserA haluaa antaa UserB käyttää tallennustilan tilin tiettyjä oikeuksilla rajoitetun ajan:

1. UserA Luo SAS, (jotka koostuvat tallennustilan tilin yhteysmerkkijonon) tietyn ajanjakson ja haluamasi oikeudet.
1. UserA jakaa Suojaussidokset haluavasi tallennustilan tilin käyttö - UserB, tämän esimerkin henkilön kanssa.  
1. UserB liittäminen kuuluvat UserA käyttämällä annettua SAS tilin tallennustilan Explorer (ennakkoversio) avulla. 

### <a name="get-a-sas-for-the-account-you-want-to-share"></a>Tili, jonka haluat jakaa SAS hankkiminen

1.  Tallennustilan Explorerissa (ennakkoversio) tallennustilan-tili, jonka haluat jakaa hiiren kakkospainikkeella ja - pikavalikosta – Valitse **Hae jaettu Access-allekirjoitus**.

    ![Hae SAS konteksti-valikkovaihtoehto][13]

1. **Jaettu Access varmenne** -valintaikkuna, valitse Määritä aikaväli ja tilin käyttöoikeudet ja valitse **Luo**.

    ![Hae SAS-valintaikkuna][14]
 
1. Toinen **Jaettu Access-varmenne** -valintaikkuna tulee näkyviin näyttäminen Suojaussidokset. Valitse **Kopioi** **Yhteysmerkkijono** , kopioi se Leikepöydälle vieressä. Valitse **Sulje** hylkää valintaikkuna.

### <a name="attach-to-the-shared-account-using-the-sas"></a>Jaetun tilillä Suojaussidokset liittäminen

1.  Valitse tallennustilan Explorerissa (ennakkoversio), **Muodosta yhteys Azure-tallennustilan**.

    ![Azure tallennuspaikka ja yhdistäminen][23]

1.  Valitse **Muodosta yhteys Azuren tallennustilaan** -valintaikkunassa Määritä yhteysmerkkijono ja valitse sitten **Seuraava**.

    ![Yhteyden muodostaminen Azure tallennustilan-valintaikkuna][24] 

1.  **Yhteyden yhteenveto** -valintaikkunassa Tarkista tiedot. Jos haluat vaihtaa, valitse **takaisin** ja kirjoita haluamasi asetukset uudelleen. Kun olet valmis, valitse **Yhdistä**.

1.  Kun liitetty, tallennustilan tilin näkyvät lukee (SAS) liitetään antamasi tilin nimi.

    ![Liitetty tili käyttämällä SAS tulos][17]

## <a name="attach-service-using-sas"></a>Liitä palvelun SAS käyttäminen

Osan [Liitä tallennustilan tilillä SAS](#attach-storage-account-using-sas) kuvataan, miten Azure tilauksen järjestelmänvalvoja voi myöntää väliaikaisen tallennustilan tilin muodostamalla (ja jakaminen) SAS tallennustilan tilin. Vastaavasti SAS voidaan luoda tallennustilan tilin tietyn palvelun (blob-säilö, jonossa tai taulukko).  

### <a name="generate-a-sas-for-the-service-you-want-to-share"></a>Luo SAS-palveluun, jonka haluat jakaa

Tässä yhteydessä palvelu voi olla blob-säilö, jonossa tai taulukko. Seuraavissa osissa kerrotaan, miten Luo Suojaussidokset luettelossa palvelulle:

- [Hanki Suojaussidokset blob-säilö](./vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
- Hanki Suojaussidokset jaetun tiedostoresurssin – *tulossa pian*
- Hae Suojaussidokset jonon – *tulossa pian*
- Hae Suojaussidokset taulukon – *tulossa pian*

### <a name="attach-to-the-shared-account-service-using-the-sas"></a>Jaetun tilin-palvelu Suojaussidokset liittäminen

1.  Valitse tallennustilan Explorerissa (ennakkoversio), **Muodosta yhteys Azure-tallennustilan**.

    ![Azure tallennuspaikka ja yhdistäminen][23]

1.  Valitse **Muodosta yhteys Azuren tallennustilaan** -valintaikkunassa Määritä SAS URI-tunnus ja valitse sitten **Seuraava**.

    ![Yhteyden muodostaminen Azure tallennustilan-valintaikkuna][24] 

1.  **Yhteyden yhteenveto** -valintaikkunassa Tarkista tiedot. Jos haluat vaihtaa, valitse **takaisin** ja kirjoita haluamasi asetukset uudelleen. Kun olet valmis, valitse **Yhdistä**.

1.  Kun liitetty, juuri liitetyn palvelun näkyvät solmun **(Service SAS)** .

    ![Tuloksena on jaettu palvelu SAS liittäminen][20]

## <a name="search-for-storage-accounts"></a>Etsi tallennustilan tilit

Jos sinulla on tallennustilan tilien luettelo on pitkä, nopea tapa etsiä tietyn tallennustilan tilin on vasemmanpuoleisen ruudun yläosassa hakuruudun avulla. 

Kun kirjoitat hakuruutuun, vasemmanpuoleisessa ruudussa näkyy vain tallennustilan tilit, jotka vastaavat enintään kirjoittamasi etsittävä arvo, joka osoittaa. Seuraavassa näyttökuvassa esitetään esimerkki, jossa I on etsitään kaikki tallennustilan tilit, joissa tallennustilan tilin nimi on teksti "tarcher".

![Tallennustilan tilin haku][11]
    
Voit tyhjentää haun valitsemalla haku-ruutuun **x** -painiketta.

## <a name="next-steps"></a>Seuraavat vaiheet
- [Resurssien Azure blob storage tallennustilan Resurssienhallinnassa (ennakkoversio)](./vs-azure-tools-storage-explorer-blobs.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-icon.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-account-link.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/subscriptions-list.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-accounts-list.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/mase.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-icon.png
[24]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-next.png
