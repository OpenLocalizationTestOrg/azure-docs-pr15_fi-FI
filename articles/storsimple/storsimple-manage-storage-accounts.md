<properties 
   pageTitle="Hallita tiliäsi tallennustilan StorSimple | Microsoft Azure"
   description="Tässä artikkelissa kerrotaan, miten voi käyttää StorSimple hallinta-Määritä sivun lisääminen, muokkaaminen ja poistaminen tai kiertää tallennustilan tilin suojaus-näppäimiä."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/29/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-your-storage-account"></a>Tallennustilan tilin hallinta-StorSimple hallinta-palvelun avulla

## <a name="overview"></a>Yleiskatsaus

**Määritä** sivun näkyy kaikki StorSimple hallintapalvelu: ssä luodut yleinen service-parametrit. Nämä parametrit voi suojata kaikki palvelun liitettyjen laitteiden ja sisältää:

- Tallennustilan tilit 
- Kaistanleveyden mallit 
- Accessin ohjausobjektin tietueiden 

Tässä opetusohjelmassa kerrotaan, miten voit käyttää **määrittäminen** -sivulla voit lisätä, muokata, tai poista tallennustilan tilit tai kiertää tallennustilan tilin suojaus-näppäimiä.

 ![Sivun määrittäminen](./media/storsimple-manage-storage-accounts/HCS_ConfigureService.png)  

Tallennustilan-tileillä tunnistetiedot, jotka laitteen käyttää tiliäsi tallennustilan cloud-palveluntarjoajalta. Microsoft Azure-tallennustilan tilit ne ovat tunnistetiedot, kuten tilin nimi ja ensisijainen pikanäppäin. 

Kaikki tallennustilan-tilit, jotka on luotu laskutuksen tilauksen näkyvät **määrittäminen** -sivulla sarakemuodossa, joka sisältää seuraavat tiedot:

- **Nimi** – liitetään tili, kun se on luotu yksilöllinen nimi.
- **SSL käytössä** – onko SSL on käytössä ja laitteen cloud viestintä on suojatun kanavan kautta.
- **Käytön mukaan** – tietomääristä tilillä tallennustilan määrä.

Yleisimpien toimintojen pikanäppäimet liittyvät tallennustilan tilit, jotka voivat tehdä **määrittäminen** -sivulla ovat seuraavat:

- Tallennustilan tilin lisääminen 
- Tallennustilan tilin muokkaaminen 
- Tallennustilan tilin poistaminen 
- Tallennustilan tilien avaimen kierto 

## <a name="types-of-storage-accounts"></a>Tallennustilan tilityypit

On kolmenlaisia tallennustilan tilit, jotka voidaan StorSimple-laitteen kanssa.

- **Automaattisesti luodut tallennustilan tilit** – kuten nimi ilmaisee, tällaista tallennustilan tilin luodaan automaattisesti palvelun luonnin yhteydessä. Lisätietoja siitä, miten tämä tallennustilan-tili on luotu, katso [Vaihe 1: Luo uusi palvelu](storsimple-deployment-walkthrough-u1.md#step-1-create-a-new-service) - [käyttöönotto paikallisen StorSimple laitteen](storsimple-deployment-walkthrough.md). 
- **Tallennustilan tilit-palvelun tilaus** – nämä ovat Azure tallennustilan tilit, jotka ovat samassa tilaukseen, palvelu. Jos haluat lisätietoja siitä, miten nämä tallennustilan luoda tiliä, saat [Tietoja Azure tallennustilan tilit](../storage/storage-create-storage-account.md). 
- **Tallennustilan tilit service-tilausta ulkopuoliset** – nämä ovat Azure tallennustilan tilit, jotka eivät liittyvä palvelusi ja todennäköisesti oli, ennen kuin palvelu on luotu.

## <a name="add-a-storage-account"></a>Tallennustilan tilin lisääminen

Voit lisätä tallennustilaa tilin antamalla yksilöllinen nimi ja tunnistetietoja, jotka on linkitetty tallennustilan tiliä (määritetyn cloud-palveluntarjoajan). Voit myös halutessasi käyttöönottoon SSL layer (SSL)-tilassa käyttämäsi laite ja pilveen väliseen viestintään verkon suojatun kanavan luominen.

Voit luoda useita tilejä annetun cloud-palveluntarjoajan. Huomaa kuitenkin, että kun tallennustilan-tili on luotu, et voi muuttaa cloud-palveluntarjoajalta.

Tallennustilan tilin tallennetaan, kun palvelua yrittää muodostaa yhteyden cloud-palveluntarjoajalta. Tunnistetietojen ja access-materiaalit, jotka olet antanut todennetaan tällä hetkellä. Tallennustilan-tili on luotu vain, jos todennus on onnistunut. Jos todennus epäonnistuu, asianmukainen virhesanoma näytetään.

Luotavien Azure-portaalissa Resurssienhallinta tallennustilan tilien tuetaan myös StorSimple. Resurssienhallinta tallennustilan tilit ei näkyy valinnan avattavasta luettelosta, kun yrität luoda aseman säilön, vain luotu Azure perinteinen portaalissa tallennustilan tilit näkyvät. Resurssienhallinta tallennustilan tilit on lisätään jäljempänä tallennustilan tilin lisääminen ohjeiden avulla.

> [AZURE.NOTE] Ohjeet tallennustilan tilin lisääminen vaihtelee käytössäsi on StorSimple versio. Varmista oikea noudattamalla StorSimple-versiosi.


[AZURE.INCLUDE [add-a-storage-account-update1](../../includes/storsimple-configure-new-storage-account-u1.md)]

[AZURE.INCLUDE [add-a-storage-account](../../includes/storsimple-configure-new-storage-account.md)]

## <a name="edit-a-storage-account"></a>Tallennustilan tilin muokkaaminen

Voit muokata tallennustilan-tili, jota käytetään aseman säilön. Jos muokkaat tallennustilan-tili, jolla on käytössä, vain kenttää voidaan muokata on tallennustilan tilin pikanäppäin. Voit antaa uusi tallennustilan pikanäppäin ja päivitetyt asetusten tallentaminen.

#### <a name="to-edit-a-storage-account"></a>Voit muokata tallennustilan-tiliä

1. Valitse service-aloitussivu palvelun, kaksoisnapsauta palvelunimi ja valitse sitten **Määritä**.

2. Valitse **Lisää/Muokkaa tallennustilan tilit**.

3. **Lisää/Muokkaa tallennustilan tilit** -valintaikkunassa:

  1. Valitse avattavasta luettelosta **Tallennustilan**tileistä, aiemmin tilille, jota haluat muokata. Tämä saattaa sisältää myös tallennustilan tilit, jotka on luotu automaattisesti palvelun luonnin yhteydessä.
  2. Tarvittaessa voit muokata **SSL-tilan ottaminen käyttöön** valinnan.
  3. Voit kiertää tallennustilan-tilin pikanäppäimet. Saat lisätietoja tekemisestä avaimen kierto [avain kiertoa tallennustilan tilit](#key-rotation-of-storage-accounts) .
  4. Valitse tarkistuksen kuvake ![kuvakkeesta](./media/storsimple-manage-storage-accounts/HCS_CheckIcon.png) Tallenna asetukset. **Määritä** sivun päivitetään asetukset. Valitse **Tallenna** Tallenna päivitettyä asetukset.

     ![Tallennustilan tilin muokkaaminen](./media/storsimple-manage-storage-accounts/HCs_AddEditStorageAccount.png)
  
## <a name="delete-a-storage-account"></a>Tallennustilan tilin poistaminen

> [AZURE.IMPORTANT] Voit poistaa tallennustilaa-tilin vain, jos se ei käytetä aseman säilön. Jos tallennustilan tilin käyttämän aseman säilön, poista ensin aseman säilö ja poista sitten liittyvä tallennustilan tilin.

#### <a name="to-delete-a-storage-account"></a>Tallennustilan tilin poistaminen

1. Valitse StorSimple hallinnan service-aloitussivu palvelun, kaksoisnapsauta palvelunimi ja valitse sitten **Määritä**.

2. Tallennustilan tilit taulukkomuotoinen luettelossa hiiren osoitinta tili, josta haluat poistaa.

3. Poista-kuvaketta (**x**) näkyvät tallennustilan tilin erittäin oikeanpuoleisessa sarakkeessa. Valitse Poista tunnistetiedot **x** -kuvaketta.

4. Kun ohjelma pyytää vahvistusta, valitse **Kyllä** poistoa. Taulukkomuotoinen luettelo päivitetään muutosten mukaiseksi.

## <a name="key-rotation-of-storage-accounts"></a>Tallennustilan tilien avaimen kierto

Tietoturvasyistä avaimen kierto on usein tietojen keskikohdan mukaan-pyyntö. 

> [AZURE.NOTE] Seuraavat avaimen kierto tiedot ja kierto-toimintosarjan koskevat vain Microsoft Azuren tallennustila-tilit. Jos käytät toisen cloud-palveluntarjoajan, voit hallita tallennustilan tilin näppäimet, kehittäjän raporttinäkymät-ikkunan kautta.
 
Microsoft Azure jokaisen tilauksen voi olla vähintään yksi liittyvä tallennustilan tilit. Näissä tileissä käyttöoikeus ohjataan tallennustilan tileille tilauksen ja access-näppäimiä. 

Kun luot tallennustilan tilin, Microsoft Azure Luo kaksi 512-bittinen tallennustilan pikanäppäimet, joita käytetään todennusta varten, kun tallennustilan tilin käytetään. On kaksi tallennustilan pikanäppäinten avulla voit näppäimet palvelukatkoksia tallennustilan-palvelussa tai käyttää kyseisen palvelun uudelleen. Varmuuskopioiden näppäimen kutsutaan *toissijaisen* avaimen avainta, joka on tällä hetkellä käytössä on *perusavain* . Yksi kaksi näppäimet määritettävä, kun Microsoft Azure StorSimple laitteen käyttää cloud tallennustilan-palveluntarjoajan.

## <a name="what-is-key-rotation"></a>Mikä on avaimen kierto?

Yleensä sovellusten käyttämään vain yksi näppäimet tiedot. Tietyn ajan kuluttua aikaa Voit määrittää sovellustesi siirtää toisen avaimen avulla. Kun olet vaihtanut sovellustesi toissijaisen avaimen, voit Keskeytä ensimmäiseksi näppäimeksi ja luo sitten uusi avain. Sovellusten pääsy tietoihin käyttämällä kahta näppäintä näin avulla niin, ettei mitään käyttökatkot.

Tallennustilan tilin näppäimet tallennetaan aina salattuja muodossa-palvelussa. Nämä voi kuitenkin palauttaa StorSimple hallintapalvelu kautta. Palvelu noutaa perusavain ja tallennustilaa tilien toissijaisen avaimen saman tilauksen, mukaan lukien tileistä tallennuspalvelu sekä oletusarvo-tallennustilan luotu luoda luodaan, kun StorSimple hallinta-palvelun on ensin tiliä. StorSimple hallinta-palvelun aina hakee painettavat näppäimet Azure perinteinen portaalin ja tallentaa ne salattuja tavalla.

## <a name="rotation-workflow"></a>Kierto-työnkulku

Microsoft Azure-järjestelmänvalvoja voi uudelleen tai muuttaa ensisijaisen tai toissijaisen avaimen käyttäminen suoraan (joko Microsoft Azuren tallennustilaan-palvelu) tallennustilan-tili. StorSimple hallintapalvelu ei näy muutoksen automaattisesti.

Ilmoittaa muutoksesta StorSimple hallintapalveluun, jota on käyttää StorSimple hallintapalvelu tallennustilan tilin käyttöä, ja synkronoida (sen mukaan, mikä on muutettu) ensisijaisen tai toissijaisen-näppäintä. Palvelun saa uusinta avainta, salaa näppäimet ja lähettää salatun avaimen laitteeseen.

#### <a name="to-synchronize-keys-for-storage-accounts-in-the-same-subscription-as-the-service-azure-only"></a>Jos haluat synkronoida tallennustilan tilit-palveluun (vain Azure) saman tilauksen näppäimet

1. Valitse **palvelut** -sivulla **Määritä** -välilehti.

2. Valitse **Lisää/Muokkaa tallennustilan tilit**.

3. Valitse-valintaikkunassa seuraavasti:

  1. Valitse tallennustilan-tili, jonka haluat synkronoida-näppäimen kanssa. Tallennustilan tilin näppäimet salataan, kun ne ovat näkyvissä.
  2. StorSimple hallintapalvelu haluat päivittää, on aiemmin muutettu Microsoft Azure tallennuspalvelu avain. Jos ensisijainen pikanäppäin on muutettu (regeneroitu), valitse **Synkronoi perusavain**. Jos toissijaisen avaimen on muutettu, valitse **Synkronoi toissijaisen avaimen**.

    ![Synkronoi näppäimet](./media/storsimple-manage-storage-accounts/HCS_KeyRotationStorageAccountSameSubscriptionAsService.png)

#### <a name="to-synchronize-keys-for-storage-accounts-outside-of-the-service-subscription"></a>Jos haluat synkronoida tallennustilan tilit service-tilausta ulkopuoliset näppäimet

1. Valitse **palvelut** -sivulla **Määritä** -välilehti.

2. Valitse **Lisää/Muokkaa tallennustilan tilit**.

3. Valitse-valintaikkunassa seuraavasti:

  1. Valitse tallennustilan-tili, jonka haluat päivittää access-näppäimen kanssa.
  2. Sinun on päivitettävä StorSimple hallintapalvelu tallennustilan pikanäppäin. Tällöin näet tallennustilan pikanäppäin. Kirjoita uusi avain **Tallennustilan tilin pikanäppäin**y-ruutuun. 
  3. Tallenna muutokset. Tallennustilan käyttö tiliavain nyt voi päivittää.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [StorSimple suojaus](storsimple-security.md).
- Lisätietoja [ammattimainen StorSimple laitteen StorSimple hallinta-palvelun avulla](storsimple-manager-service-administration.md).
