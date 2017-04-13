<properties
    pageTitle="Tietoja Azure ulkoisen palvelun maksuja | Microsoft Azure"
    description="Lisätietoja ulkopuolisiin palveluihin, aikaisemmin Marketplacen Azure kulujen laskutus."
    services=""
    documentationCenter=""
    authors="adpick"
    manager="felixwu"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="adpick"/>

# <a name="understand-your-azure-external-service-charges"></a>Azure ulkoisen palvelun maksuja ymmärtäminen

Tässä artikkelissa kerrotaan laskutuksen ulkoisten palvelujen Azure-tietokannassa. Ulkoisten palvelujen avulla voidaan kutsua Marketplace-tilauksia. Ulkoisten palvelujen riippumaton palvelun toimittajien tarjoamien, mutta tulevat näkyviin kokonaan Azure ekosysteemissä kuluessa. Lue, miten voit:

- Ulkoisten palvelujen määrittäminen
- Tietoja siitä, miten Laskutus eroaa Azure muut resurssit
- Käytä ulkoisia palveluja kaikki jaksotetaan kustannukset projektin aikataulun hahmottaminen
- Ulkoiseen palveluun tilaukset ja miten niiden maksat hallinta

## <a name="what-are-azure-external-services"></a>Mitkä ovat Azure ulkoisia palveluja?

Ulkoisten palvelujen avulla voidaan kutsua Azure Marketplacesta. Yleensä ne palvelut, jotka on julkaistu ulkopuolisten käytettävissä Azure. Esimerkiksi ClearDB ja SendGrid ovat ulkoisia palveluja, jotka voivat hankkia Azure, mutta ei julkaista Microsoft.

### <a name="identify-external-services"></a>Ulkoisten palvelujen määrittäminen

Voit valmistella uuden ulkoisen palvelun tai resurssi, näkyy ilmoitus:

![Marketplace ostaa varoitus](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

>[AZURE.NOTE] Ulkoisten palvelujen julkaistaan yritykset, jotka eivät ole Microsoft, mutta joskus Microsoft-tuotteiden luokitellaan myös ulkoisten palvelujen seuraavasti.

### <a name="external-services-are-billed-separately"></a>Ulkoisten palvelujen ovat laskuttaa erikseen

Ulkoisten palvelujen käsitellään yksittäisten tilausten Azure-tilauksen piiriin kuuluvien. Jokaisen palvelun laskutusjaksolla määritetään, kun olet ostanut palvelun. Ei sekoittaa tilaus, jonka ostit sen laskutusjaksolla. Saat myös erillinen laskut ja luottokorttisi veloitetaan erikseen.

### <a name="each-external-service-has-a-different-billing-model"></a>Ulkoinen jokaisella palvelulla on eri laskutuksen mallia

Joidenkin palvelujen ovat laskuttaa tukiyhteyksiä tavalla, kun muut käyttävät kuukausittain perusteella maksu-malli. Tarvitset luottokorttia Azure ulkoisia palveluja, ei voi ostaa ulkoisten palvelujen kanssa laskulla maksaminen.

### <a name="you-cant-use-monthly-free-credits-for-external-services"></a>Kuukausittaiset vapaa käyttöoikeudet voi käyttää ulkoisia palveluja

Jos käytössäsi on Azure tilauksesta, joka sisältää [vapaa hyvitykset](https://azure.microsoft.com/pricing/spending-limits/), niitä ei voi käyttää ulkoisen palvelun laskut. Osta ulkoisia palveluita luottokorttia avulla.

## <a name="view-external-service-spending-and-history"></a>Näytä ulkoisen palvelun tehtäviinsä ja historia

Voit tarkastella luetteloa ulkoisia palveluja, jotka ovat jokaisen tilauksen [Azure portaalissa](https://portal.azure.com/): 

1. Kirjaudu [Azure-portaaliin](https://portal.azure.com/) ja [Siirry kohtaan **Laskutus** -sivu](https://portal.azure.com/?flight=1#blade/Microsoft_Azure_Billing/BillingBlade).

    ![Valitse Laskutus-toiminnossa-valikko](./media/billing-understand-your-azure-marketplace-charges/billing-button.png) 
  
2. Valitse **tilauksen kustannukset** -osassa, jota haluat tarkastella tilaus. 
   
    ![Valitse tilaus laskutuksen sivu](./media/billing-understand-your-azure-marketplace-charges/select-sub.png)

3. Valitse **ulkoinen palvelut**.

    ![Valitse ulkoinen palvelut tilaus-sivu](./media/billing-understand-your-azure-marketplace-charges/external-service-blade.png)

4. Raportissa pitäisi näkyä eri ulkoisen service-tilaukset, julkaisijan nimi, olet ostanut palvelutaso, annoit resurssi ja tilauksen nykyisen tilan. Valitse ulkoinen palvelu, voit tarkastella aiempia laskut.

    ![Valitse ulkoinen palvelu](./media/billing-understand-your-azure-marketplace-charges/external-service-blade2.png)

5. Tässä kohdassa voit tarkastella aiempia laskun summat mukaan lukien ALV-erittelyn.

    ![Näytä ulkoisen palvelut Laskuhistorian](./media/billing-understand-your-azure-marketplace-charges/billing-overview-blade.png)

## <a name="manage-payment-methods-for-external-service-orders"></a>Maksutapojen ulkoiseen palveluun tilausten hallinta

Päivitä ulkoinen palvelun tilausten [Tilin Center](https://account.windowsazure.com/)Maksutapasi.

> [AZURE.NOTE] Jos ostit tilauksen työpaikan tai oppilaitoksen tilillä kannattaa tehdä muutoksia maksutavan [tuelta](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .

1. Kirjaudu sisään [Tilille Center](https://account.windowsazure.com/) ja [Siirry **marketplace** -välilehti](https://account.windowsazure.com/Store)

    ![Valitse tilin keskelle marketplace](./media/billing-understand-your-azure-marketplace-charges/select-marketplace.png)

2. Valitse ulkoinen palvelu haluat hallita

    ![Valitse ulkoinen palvelu haluat hallita](./media/billing-understand-your-azure-marketplace-charges/select-ext-service.png)

3. Valitse sivun oikeassa reunassa **Vaihda maksutapa** . Tämä linkki näyttää hallittavan maksutavan eri portaaliin.
    
    ![Tilauksen yhteenveto](./media/billing-understand-your-azure-marketplace-charges/change-payment.PNG)

4. Valitse **Muokkaa tiedot** ja noudata ohjeita päivittää maksutiedot.

    ![Valitse Muokkaa tiedot](./media/billing-understand-your-azure-marketplace-charges/edit-info.png)
    
## <a name="cancel-an-external-service-order"></a>Ulkoinen service-tilauksen peruuttaminen

Jos haluat peruuttaa tilauksen ulkoiseen palveluun haluat poistaa [Azure portal](https://portal.azure.com)resurssi.

![Poista resurssi](./media/billing-understand-your-azure-marketplace-charges/deleteMarketplaceOrder.PNG)

## <a name="need-help-contact-support"></a>Tarvitsetko apua? Ota yhteyttä tukeen.

Jos on edelleen muita kysymyksiä, ota [yhteyttä tukeen](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ongelmaa saat ratkaistu nopeasti.
