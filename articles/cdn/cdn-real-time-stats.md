<properties
    pageTitle="Real-aikainen – tilasto-Azure CDN | Microsoft Azure"
    description="Reaaliaikainen tilasto on reaaliaikaisia tietoja Azure CDN suorituskyvystä pitäessäsi sisällön asiakkaat."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="real-time-stats-in-microsoft-azure-cdn"></a>Reaaliaikainen tilasto-Microsoft Azure CDN

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Yleiskatsaus

Tässä asiakirjassa kerrotaan reaaliaikainen tilasto-Microsoft Azure CDN.  Tämä toiminto on reaaliaikaisia tietoja, kuten kaistanleveys, välimuistin tilat ja samanaikaisia yhteyksiä CDN profiiliisi pitäessäsi sisällön asiakkaat. Näin palvelun kunto jatkuva seuranta milloin tahansa, mukaan lukien tarkistusluettelo tapahtumat.

Seuraavat kaaviot ovat käytettävissä:

* [Kaistanleveyden](#bandwidth)
* [Tilakoodin](#status-codes)
* [Välimuistin tilat](#cache-statuses)
* [Yhteydet](#connections)


## <a name="accessing-real-time-stats"></a>Reaaliaikainen tilasto käyttäminen

1. Siirry [Azure Portal](https://portal.azure.com)CDN profiilin.

    ![CDN profiili-sivu](./media/cdn-real-time-stats/cdn-profile-blade.png)

2. Valitse **hallinta** -painike CDN profiili-sivu.

    ![CDN profiilin sivu hallinta-painike](./media/cdn-real-time-stats/cdn-manage-btn.png)

    CDN hallinta-portaalin avautuu.

3. Vie hiiren osoitin **Analytics** -välilehti ja valitse Vie hiiren osoitin **Reaaliaikaisen tilasto** -valikon avauspainike.  Valitse **HTTP objekti**.

    ![CDN hallinta-portaalin](./media/cdn-real-time-stats/cdn-premium-portal.png)

    Reaaliaikainen tilasto-kaaviot ovat näkyvissä.
    
Kunkin kaavioiden näyttää valitun loppupäivät, alkaen sivun latautuessa reaaliaikainen tilastotiedot.  Kaavioiden päivittyvät automaattisesti muutaman sekunnin välein.  **Päivitä** kaaviopainike, jos sellainen tyhjentää kaavio, jonka jälkeen näkyviin tulevat vain valitut tiedot.

## <a name="bandwidth"></a>Kaistanleveyden

![Kaistanleveyden graph](./media/cdn-real-time-stats/cdn-bandwidth.png)

**Kaistanleveyden** kaavio näyttää kaistanleveyden käytettäviä nykyinen ympäristö valitun loppupäivät päälle. Kaavio Varjostettu osuus ilmaisee kaistanleveyden käytön. Alapuolella viivakaaviota näytetään käytössä kaistanleveyden tarkka summa.

## <a name="status-codes"></a>Tilakoodin

![Koodin tilakaavio](./media/cdn-real-time-stats/cdn-status-codes.png)

**Tilakoodin** kaavio osoittaa, kuinka usein tiettyjä HTTP-vastauksen koodit esiintyvien valitun loppupäivät päälle.

> [AZURE.TIP]  Kunkin HTTP tila koodi-vaihtoehdon kuvaus-kohdassa [Azure CDN HTTP-tilakoodin](https://msdn.microsoft.com/library/mt759238.aspx).

HTTP-tilakoodin luettelo näkyy suoraan kaavion yläpuolelle. Tämän luettelon osoittaa kunkin tilakoodi, joka voidaan lisätä viivakaaviota ja esiintymien sekunnissa kyseisen tilakoodin nykyisen määrän. Oletusarvon mukaan rivi näkyy kunkin Graphiin nämä tilakoodit. Voit kuitenkin valvoa vain tilakoodit, jotka on erityinen merkitys CDN kokoonpanon. Voit tehdä tämän haluamasi tilakoodin Tarkista ja poista kaikki valinnat ja valitse sitten **Päivitä kaavion**. 

Voit piilottaa tilapäisesti tietyn tilakoodin kirjaustiedot.  Napsauta kaavion alla olevassa selitteen haluat piilottaa tilakoodi. Tilakoodin piilotetaan heti kaavio. Napsauttamalla kyseisen tilakoodin uudelleen aiheuttaa asetusta näytetään uudelleen.

## <a name="cache-statuses"></a>Välimuistin tilat

![Välimuistin tilat graph](./media/cdn-real-time-stats/cdn-cache-status.png)

**Välimuistin tilat** kaavio osoittaa, kuinka usein välimuistin tilat tietyntyyppisiä määrä valitun loppupäivät päälle. 

> [AZURE.TIP]  Kunkin välimuistin tila koodi-vaihtoehdon kuvaus-kohdassa [Azure CDN välimuistin tilakoodin](https://msdn.microsoft.com/library/mt759237.aspx).

Välimuistin tilakoodin luettelo näkyy suoraan kaavion yläpuolelle. Tämän luettelon osoittaa kunkin tilakoodi, joka voidaan lisätä viivakaaviota ja esiintymien sekunnissa kyseisen tilakoodin nykyisen määrän. Oletusarvon mukaan rivi näkyy kunkin Graphiin nämä tilakoodit. Voit kuitenkin valvoa vain tilakoodit, jotka on erityinen merkitys CDN kokoonpanon. Voit tehdä tämän haluamasi tilakoodin Tarkista ja poista kaikki valinnat ja valitse sitten **Päivitä kaavion**. 

Voit piilottaa tilapäisesti tietyn tilakoodin kirjaustiedot.  Napsauta kaavion alla olevassa selitteen haluat piilottaa tilakoodi. Tilakoodin piilotetaan heti kaavio. Napsauttamalla, että tilakoodin uudelleen aiheuttaa asetusta näytetään uudelleen.

## <a name="connections"></a>Yhteydet

![Yhteydet-kaavio](./media/cdn-real-time-stats/cdn-connections.png)

Tämä kaavio osoittaa, kuinka monta yhteyksiä muodostettiin reuna-palvelimiin. Kunkin pyyntö resurssi, joka välittää Microsoftin CDN tulokset-yhteyden kautta.

## <a name="next-steps"></a>Seuraavat vaiheet

- Saat ilmoituksen kanssa [reaaliajassa ilmoitukset Azure CDN](cdn-real-time-alerts.md)
- Tarkastella tarkemmin – [Lisäasetukset HTTP](cdn-advanced-http-reports.md) -raportit
- Analysoi [käyttötavat](cdn-analyze-usage-patterns.md)

