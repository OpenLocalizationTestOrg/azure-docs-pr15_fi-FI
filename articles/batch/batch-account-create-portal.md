<properties
    pageTitle="Luo tili Azure erä | Microsoft Azure"
    description="Azure erä-tilin luominen suorittaa suurissa rinnakkain työmääriä pilveen Azure-portaalissa"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/21/2016"
    ms.author="marsma"/>

# <a name="create-an-azure-batch-account-using-the-azure-portal"></a>Azure-portaalissa Azure erä tilin luominen

> [AZURE.SELECTOR]
- [Azure portal](batch-account-create-portal.md)
- [Erän hallinta .NET](batch-management-dotnet.md)

Azure erä-tilin luominen [Azure portal][azure_portal], ja etsiä tärkeää tilin ominaisuudet, kuten valintanäppäimet ja tilin URL-osoitteet. Käsiteltävät aiheet myös erä hinnat ja Azure-tallennustilan tilin linkittäminen erä-tilisi, niin, että voit käyttää [sovelluksen pakettien](batch-application-packages.md) ja [jatkuvat työn ja tehtävän tulos](batch-task-output.md).

## <a name="create-a-batch-account"></a>Erän tilin luominen

1. Kirjautuminen [Azure portal][azure_portal].

2. Valitse **Uusi** > **Laske** > **erä palvelu**.

    ![Siirtoerän Marketplacesta][marketplace_portal]

3. **Erän uusi tili** -sivu tulee näkyviin. Katso kohteiden ** – *e* kuvaukset sivu jokaisen osan.

    ![Erän tilin luominen][account_portal]

    a. **Tilin nimi**: erä tilisi yksilöllinen nimi. Tämä nimi on oltava yksilöllinen Azure alue tili on luotu (katso *sijainti* alla). Se voi olla vain pieniä kirjaimia, numeroita ja on oltava 3 – 24 merkkiä.

    b. **Tilaus**: tilaus, johon haluat luoda erää-tilin. Jos käytössäsi on vain yksi tilaus, se on oletusarvoisesti valittuna.

    c-näppäinyhdistelmää. **Resurssiryhmä**: aiemmin resurssin uuden Siirtoerän tilin ryhmän tai voit myös luoda uuden.

    d. **Sijainti**: Azure-alue, johon haluat luoda erää-tilin. Vain tilauksen ja resurssiryhmä tukemat alueet näkyvät asetukset.

    e. **Tallennustilan tilin** (valinnainen): **yleisiä tarkoitus** tallennustilan tilin-tiliisi erä liittää (linkki). Saat lisätietoja [linkitetyn Azure-tallennustilan tilin](#linked-azure-storage-account) alapuolella.

4. Valitse **Luo** Luo tili.

  Portaalin ilmaisee, että on **käyttöönotto** tili ja valmistumisen- **käyttöönotoissa onnistui** -ilmoitus näkyy *ilmoitukset*.

## <a name="view-batch-account-properties"></a>Erän tilin ominaisuuksien tarkasteleminen

Kun tili on luotu, voit avata **erä tili-sivu** , voit käyttää sen asetukset ja ominaisuudet. Voit käyttää kaikkia tilin asetukset ja ominaisuudet erä tili-sivu vasemmasta valikosta.

![Erän tili-sivu Azure-portaalissa][account_blade]

* **Erän tilin URL-osoite**: [erä kehittäminen ohjelmointirajapinnan](batch-technical-overview.md#batch-development-apis) sovellusten on tili URL-osoitteen resurssien hallintaa ja töiden suorittaminen tili. Erän tilin URL-osoite on seuraavanlainen:

    `https://<account_name>.<region>.batch.azure.com`

![Erän tilin URL-osoite-portaalissa][account_url]

* **Pikanäppäimet**: sovellustesi on myös pikanäppäin työskenneltäessä erä-tilisi tiedoilla. Voit tarkastella tai erä-tilisi pikanäppäinten uudelleen, kirjoita `keys` **vasemmanpuoleisessa valikossa hakukenttään-erä tili-sivu,** Valitse **avaimet**.

    ![Erän tilin näppäimet Azure-portaalissa][account_keys]

## <a name="pricing"></a>Hinnat

Erän tilit on käytettävissä vain "Vapaa taso," toisin sanoen ei peri erä tilin itse. Peritään pohjana Azure Laske resurssit, joiden erän ratkaisujen tarjoaman ja että työmääriä suoritettaessa muissa palveluissa kulutettu resurssien. Esimerkiksi voit peritään Laske solmujen oman jakavat ja tietojen voit tallentaa Azuren tallennustilaan input tai output tehtäviin. Vastaavasti, jos käytät erän [sovelluksen paketit](batch-application-packages.md) -ominaisuutta, perittävän tallennetaan sovelluksen pakettien Azuren tallennustilaan resurssit. Katso [erä hinnat] [ batch_pricing] lisätietoja.

## <a name="linked-azure-storage-account"></a>Azuren tallennustilaan linkitetty asiakas

Kuten edellä mainittiin, voit linkittää **yleisiä tarkoitus** tallennustilan tilin erä tilisi (valinnainen). Erän [sovelluksen pakettien](batch-application-packages.md) -ominaisuus käyttää Blob-objektien tallennustilaan linkitetyn yleisiä tarkoitus tallennustilan tilin, kuten [Erä tiedoston nimeämiskäytännön .NET](batch-task-output.md) -kirjasto. Seuraavia valinnaisia ominaisuuksia auttaa erän tehtävien suorittaminen sovellusten käyttöönotto ja toteaa fonttinsa tiedot.

Erän tällä hetkellä tukee *vain* **yleisiä tarkoitus** tallennustyyppi tilin vaiheessa 5, [Luo tallennustilan tili](../storage/storage-create-storage-account.md#create-a-storage-account), [tietoja Azure](../storage/storage-create-storage-account.md)-tallennustilan tilin kuvatulla tavalla. Kun linkität Azure-tallennustilan tilin erä-tiliisi, olla että linkin *vain* **yleisiä tarkoitus** tallennustilan tili.

!["Yleisiä tarkoitus-tallennustilan tilin luominen][storage_account]

On suositeltavaa luoda käyttöönsä tallennustilan tilin erä-tili.

>[AZURE.WARNING] Varovainen, kun pikanäppäimet linkitetyn tallennustilan tilin uudelleen. Vain yksi tallennustilan tilin avain uudelleen ja valitse sitten **Synkronoi näppäimet** linkitetyn tallennustilan tili-sivu. Odota viisi minuuttia, jotta näppäimet välittäminen oman jakavat Laske solmujen sitten uudelleen ja synkronoida sitten näppäintä tarvittaessa. Jos molemmat avaimet uudelleen samaan aikaan, Laske-solmut ei voi synkronoida joko avain ja ne menettävät käyttöoikeutensa tallennustilan-tilille.

  ![Tallennustilan tilin näppäimet uudelleen][4]

## <a name="batch-service-quotas-and-limits"></a>Erän palvelun kiintiön ja rajoitukset

Ota huomioon, että Azure-tilauksesi ja Azure muiden tiettyjen [kiintiön ja rajoitukset](batch-quota-limit.md) ottaa käyttöön erä. Nykyinen kiintiön erä tilin näkyvät tilin **Ominaisuudet**-portaalissa.

![Erän tilin kiintiön Azure-portaalissa][quotas]

Ota huomioon nämä kiintiöt ovat suunnitteleminen ja määrittäminen erä työmääriä skaalaus. Esimerkiksi jos lisääminen resurssivarantoon ei ole kurottavat Laske solmujen määrittämäsi kohde määrä, voi luet core-kiintiön erä tilissäsi.

Huomaa, että et ovat ole rajoittanut erä tilistä Azure tilauksen. Voit suorittaa useita erä työmääriä erä tilistä tai jakaa oman työmääriä erä tilit saman tilauksen, mutta erilaisia Azure alueilla.

Monia näistä kiintiön voidaan lisätä vain lähetetty Azure-portaalissa ilmainen tuote tukipyynnön kanssa. Lisätietoja [kiintiöiden ja Azure erä palvelun rajoitukset](batch-quota-limit.md) pyytämisestä kiintiön kasvaa.

## <a name="other-batch-account-management-options"></a>Muut erä Tiliasetukset hallinta

Lisäksi Azure portaalin, voit myös luoda ja hallita erä tili ja seuraavasti:

* [Erän PowerShellin cmdlet-komennot](batch-powershell-cmdlets-get-started.md)
* [Azure CLI](../xplat-cli-install.md)
* [Erän hallinta .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Seuraavat vaiheet

* Artikkelissa [Azure erän ominaisuuden yhteenveto](batch-api-basics.md) lisätietoja erä palvelun käsitteitä ja toiminnot. On artikkelissa käsitellään ensisijainen erä resurssit, kuten jakavat, Laske solmujen, projektien ja tehtävien ja esitetään yleiskatsaus palvelun ominaisuuksista, jotka mahdollistavat suurissa Laske työmäärää suorittamisen.

* Opi kehittäminen käytössä-sovelluksesta valitsemalla [erä .NET asiakkaan kirjastoon](batch-dotnet-get-started.md). [Johdanto artikkelissa](batch-dotnet-get-started.md) opastaa toimimasta-sovellus, joka käyttää suorittamiseen työmäärä useita Laske solmuissa erä-palvelun ja sisältää Azuren tallennustilaan käyttäminen kuormituksen tiedoston väliaikaisen ja hakemista.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "Tallennustilan tilin näppäimet uudelleen"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[quotas]: ./media/batch-account-create-portal/quotas.png
