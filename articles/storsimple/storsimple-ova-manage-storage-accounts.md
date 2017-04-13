<properties 
   pageTitle="Hallita tiliäsi tallennustilan StorSimple | Microsoft Azure"
   description="Tässä artikkelissa kerrotaan, miten voit käyttää StorSimple hallinta-Määritä sivun lisääminen, muokkaaminen, poistaminen ja Kierrä StorSimple Virtual matriisin liittyvät tallennustilan tilin suojaus-näppäimiä."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="09/29/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-storage-accounts-for-storsimple-virtual-array"></a>Tallennustilan tilien hallinta StorSimple Virtual matriisin StorSimple hallinta-palvelun avulla

## <a name="overview"></a>Yleiskatsaus

**Määritä** sivun näkyy StorSimple hallintapalvelu: ssä luodut yleinen palveluparametrit. Nämä parametrit voi suojata kaikki palvelun liitettyjen laitteiden ja sisältää:

- Tallennustilan tilit 
- Accessin ohjausobjektin tietueiden 

Tässä opetusohjelmassa kerrotaan, miten voit käyttää **Määritä** sivun lisääminen, muokkaaminen ja poistaminen tallennustilan tilit StorSimple-Virtual matriisin. Tässä opetusohjelmassa tiedot koskevat vain maaliskuussa 2016 GA release käyttö StorSimple Virtual matriisi.

 ![Sivun määrittäminen](./media/storsimple-ova-manage-storage-accounts/configure_service_page.png)  

Tallennustilan-tileillä tunnistetiedot, jotka laitteen käyttää tiliäsi tallennustilan cloud-palveluntarjoajalta. Microsoft Azure-tallennustilan tilit ne ovat tunnistetiedot, kuten tilin nimi ja ensisijainen pikanäppäin. 

Kaikki tallennustilan-tilit, jotka on luotu laskutuksen tilauksen näkyvät **määrittäminen** -sivulla sarakemuodossa, joka sisältää seuraavat tiedot:

- **Nimi** – liitetään tili, kun se on luotu yksilöllinen nimi.
- **SSL käytössä** – onko SSL on käytössä ja laitteen cloud viestintä on suojatun kanavan kautta.

Yleisimpien toimintojen pikanäppäimet liittyvät tallennustilan tilit, jotka voivat tehdä **määrittäminen** -sivulla ovat seuraavat:

- Tallennustilan tilin lisääminen 
- Tallennustilan tilin muokkaaminen 
- Tallennustilan tilin poistaminen 


## <a name="types-of-storage-accounts"></a>Tallennustilan tilityypit

On kolmenlaisia tallennustilan tilit, jotka voidaan StorSimple-laitteen kanssa.

- **Automaattisesti luodut tallennustilan tilit** – kuten nimi ilmaisee, tällaista tallennustilan tilin luodaan automaattisesti palvelun luonnin yhteydessä. Katso lisätietoja siitä, miten tämä tallennustilan-tili on luotu, [Luo uusi palvelu](storsimple-ova-manage-service.md#create-a-service). 
- **Tallennustilan tilit-palvelun tilaus** – nämä ovat Azure tallennustilan tilit, jotka ovat samassa tilaukseen, palvelu. Jos haluat lisätietoja siitä, miten nämä tallennustilan luoda tiliä, saat [Tietoja Azure tallennustilan tilit](../storage/storage-create-storage-account.md). 
- **Tallennustilan tilit service-tilausta ulkopuoliset** – nämä ovat Azure tallennustilan tilit, jotka eivät liittyvä palvelusi ja todennäköisesti oli, ennen kuin palvelu on luotu.

Kunkin StorSimple Virtual matriisin Luo liittyvät tallennustilan tilin säilön (jonka etuliite hcs). Sisältää kaikki cloud laitteen tiedot. Älä poista tämä säilö muodostamalla yhteyden Azuren tallennustilaan-palvelun kautta, kun tämä toiminto aiheuttaa tietojen menettämisen.

## <a name="add-a-storage-account"></a>Tallennustilan tilin lisääminen

Voit lisätä tallennustilan tilin StorSimple hallinta-palvelun kokoonpanon antamalla yksilöllinen nimi ja tunnistetietoja, jotka on linkitetty tallennustilan tilin. Voit myös halutessasi käyttöönottoon SSL layer (SSL)-tilassa käyttämäsi laite ja pilveen väliseen viestintään verkon suojatun kanavan luominen.

Voit luoda useita tilejä annetun cloud-palveluntarjoajan. Tallennustilan tilin tallennetaan, kun palvelua yrittää muodostaa yhteyden cloud-palveluntarjoajalta. Tunnistetietojen ja access-materiaalit, jotka olet antanut todennetaan tällä hetkellä. Tallennustilan-tili on luotu vain, jos todennus on onnistunut. Jos todennus epäonnistuu, asianmukainen virhesanoma näytetään.

Luotavien Azure-portaalissa Resurssienhallinta tallennustilan tilien tuetaan myös StorSimple. Resurssienhallinta tallennustilan tilit ei näy valinnan, vain Azure perinteinen portaalissa luonut tilit näkyvät tallennustilan avattavasta luettelosta. Resurssienhallinta tallennustilan tilit on lisättävä Lisää tallennustilaa tili alla kuvatulla tavalla.

Perinteinen Azure-tallennustilan tilin lisääminen ohjeet on kuvattu alla.

[AZURE.INCLUDE [add-a-storage-account](../../includes/storsimple-ova-configure-new-storage-account.md)]

## <a name="edit-a-storage-account"></a>Tallennustilan tilin muokkaaminen

Voit muokata laitteen käyttämä tallennustila-tili. Jos muokkaat tallennustilan-tili, jolla on käytössä, muokkaamiseen käytettävissä olevat kentät ovat pikanäppäin ja tallennustilaa tilin SSL-tilassa. Voit Anna uusi tallennustilan pikanäppäin tai muokata **käyttöön SSL-tila** -valintaa ja Tallenna päivitetty asetukset.

#### <a name="to-edit-a-storage-account"></a>Voit muokata tallennustilan-tiliä

1. Valitse service-aloitussivu palvelun, kaksoisnapsauta palvelunimi ja valitse sitten **Määritä**.

2. Valitse **Lisää/Muokkaa tallennustilan tilit**.

3. **Lisää/Muokkaa tallennustilan tilit** -valintaikkunassa:

  1. Valitse avattavasta luettelosta **Tallennustilan**tileistä, aiemmin tilille, jota haluat muokata. 
  2. Tarvittaessa voit muokata **SSL-tilan ottaminen käyttöön** valinnan.
  3. Voit valita uudelleen tallennustilan-tilin pikanäppäimet. Lisätietoja [tallennustilan tilin näppäimet uudelleen](storage-create-storage-account.md#manage-your-storage-access-keys). Lisää uusi tallennustilan tilin-avain. Azure-tallennustilan tilin on ensisijainen pikanäppäin. 
  4. Valitse tarkistuksen kuvake ![kuvakkeesta](./media/storsimple-ova-manage-storage-accounts/checkicon.png) Tallenna asetukset. **Määritä** sivun päivitetään asetukset. 
  5. Sivun alareunassa valitsemalla **Tallenna** Tallenna päivitettyä asetukset. 

     ![Tallennustilan tilin muokkaaminen](./media/storsimple-ova-manage-storage-accounts/modifyexistingstorageaccount.png)
  
## <a name="delete-a-storage-account"></a>Tallennustilan tilin poistaminen

> [AZURE.IMPORTANT] Voit poistaa tallennustilaa-tilin vain, jos se ei ole käytössä. Jos tallennustilaa-tilisi on käytössä, saat tiedon.

#### <a name="to-delete-a-storage-account"></a>Tallennustilan tilin poistaminen

1. Valitse StorSimple hallinnan service-aloitussivu palvelun, kaksoisnapsauta palvelunimi ja valitse sitten **Määritä**.

2. Tallennustilan tilit taulukkomuotoinen luettelossa hiiren osoitinta tili, josta haluat poistaa.

3. Poista-kuvaketta (**x**) näkyvät tallennustilan tilin erittäin oikeanpuoleisessa sarakkeessa. Valitse Poista tunnistetiedot **x** -kuvaketta.

4. Kun ohjelma pyytää vahvistusta, valitse **Kyllä** poistoa. Taulukkomuotoinen luettelo päivitetään muutosten mukaiseksi.

5. Sivun alareunassa valitsemalla **Tallenna** Tallenna päivitettyä asetukset.


## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja hallinnasta [StorSimple Virtual matriisin](storsimple-ova-web-ui-admin.md).
