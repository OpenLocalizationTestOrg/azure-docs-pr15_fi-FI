<properties 
   pageTitle="Cloud Resurssienhallinnassa Azure resurssien hallinta | Microsoft Azure"
   description="Opi pilveen Resurssienhallinnan avulla voit selata ja Azure resurssien Visual Studion."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="managing-azure-resources-with-cloud-explorer"></a>Cloud Resurssienhallinnassa Azure resurssien hallinta

##<a name="overview"></a>Yleiskatsaus

Cloud Explorer on suunniteltu avulla voit Lisää helposti ja nopeasti selata ja hallita Azure resursseja Visual Studio IDE kuluessa. Voit, esimerkiksi avulla Avaa verkkosovellukseen [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) tai selaimessa, liittää virheenkorjaus, tai voit tarkastella blob-säilö ominaisuudet ja avata sen Blob säilö editorissa.

Cloud Explorer muodostanut Azure resurssien hallinnan pinossa, aivan kuten [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). Ymmärtää resurssit, kuten Azure resurssiryhmiä ja Azure-palvelut, kuten logiikan sovellukset ja API-sovellukset, ja se tukee [Roolipohjainen käyttöoikeuksien valvonta](./active-directory/role-based-access-control-configure.md) (RBAC). Voit tarkastella Azure resurssit, jotka olet lisännyt tai muuttanut valitsemalla Cloud Explorerin työkalurivin **Päivitä** -painiketta.

Cloud Explorer asennetaan osana Visual Studio Tools Azure SDK 2.7. 

## <a name="prerequisites"></a>Edellytykset

- Visual Studio 2015 RTM.

- Azure SDK Visual Studio-työkalut. 
- On myös oltava Azure-tili ja kirjautunut siihen voi tarkastella Azure resurssien Cloud Explorerissa. Jos sinulla ei ole, voit luoda tiliä vain muutaman minuutin. Jos sinulla on MSDN-tilaus, katso [Azure etu MSDN-tilaajille](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Muussa tapauksessa on [ilmainen kokeiluversio-tilin luominen](https://azure.microsoft.com/pricing/free-trial/).

- Jos Cloud Explorer ei näy, voit tarkastella sitä valitsemalla **Näytä**-valikossa **Muut Windows** **Cloud Explorer** .

## <a name="manage-azure-accounts-and-subscriptions"></a>Azure tilit ja tilausten hallinta

Cloud Explorerissa Azure resurssien näkyviin haluat Azure-tili on yksi tai useampi active tilausta kirjautuminen. Jos sinulla on useampi kuin yksi Azure-tili, voit lisätä ne Cloud Resurssienhallinnassa ja valitse sitten resurssin Cloud Resurssienhallintanäkymä sisällytettävien tilaukset.

Jos et ole käyttänyt Azure ennen tai et ole lisännyt tarvittavat tilit Visual Studiossa, sinua pyydetään tekemään niin.

## <a name="to-add-azure-accounts-to-cloud-explorer"></a>Jos haluat lisätä Azure sähköpostitilejä Cloud Explorer

1. Valitse Cloud Explorerin työkalurivin asetukset-kuvake.

1. Valitse **Lisää tili** -linkki. Kirjaudu Azure-tili, jonka resursseja, jota haluat selata. Tilin valitsin avattavassa luettelossa pitäisi olla valittuna äsken lisäämääsi tiliin. Tilin tilaukset näkyvät kohdassa tilin tapahtuma.

    ![Azure tilaukset lisääminen](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819514.png)

    ![Azure tilaukset valitseminen](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819515.png)

1. Valitse Selaa ja valitse sitten **Käytä** -painike tilin tilauksissa valintaruudut.

    Azure resurssien valitut tilaukset näkyvät Cloud hallinnassa.

## <a name="to-remove-an-azure-account"></a>Jos haluat poistaa Azure-tili

1. Valitse **Tiedosto**-valikossa **Tiliasetukset** .

1. Valitse **Tilin asetukset** -valintaikkunassa **Kaikki tilit** -osassa **Poista** komento tilin vieressä haluat poistaa. Huomaa, että toiminto poistaa vain tilin Visual Studio – se ei vaikuta itse Azure-tili.

## <a name="view-resource-types-or-groups"></a>Näytä resurssityypit tai ryhmiä

Voit tarkastella Azure resursseja, voit valita **Resurssityypit** tai **Resurssiryhmät** -näkymässä.

![Resurssin avattavasta luettelosta](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819516.png)

- **Resurssityypit** näkymä, jossa on myös [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)yleisiä näkymä, näyttää luokiteltu tyypin, kuten web Apps-sovellusten ja tallennustilaa tilit näennäiskoneiden Azure resurssit. Tämä muistuttaa miten Azure resurssien näkyvät palvelimen hallinnassa.

- Ryhmien resurssinäkymässä luokittelee Azure resurssit niihin liittyvät Azure resurssiryhmän mukaan.

 
    Resurssiryhmä on paljon Azure resurssit, yleensä käyttävät tietylle sovellukselle. Lisätietoja Azure resurssiryhmät-kohdassa [Azure resurssin hallinnassa: yleiskatsaus](./resource-group-overview.md).

## <a name="view-and-navigate-resources"></a>Tarkastella ja siirtyä resurssit

Siirry Azure resurssi ja tarkastella sen tietoja Cloud Resurssienhallinnassa, laajenna kohteen tyyppi tai liittyvän resurssiryhmä ja valitse sitten resurssi. Kun valitset resurssin, tiedot tulevat näkyviin kaksi välilehteä Cloud Explorer alareunassa.

![Valitse resurssinäkymä](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819517.png)

- **Toiminnot** -välilehti näkyy toiminnot, voit tehdä Cloud Explorer valitun resurssin. Näet myös käytettävissä olevat toiminnot resurssin pikavalikosta.

- **Ominaisuudet** -välilehti näkyy resurssi, kuten siihen liittyvä tyyppi-, kieli- ja resurssin ryhmässä ominaisuudet.

Joka resurssilla on **avoinna portaalissa**toiminnon. Kun valitset tämän toiminnon, Cloud Explorer näyttää valitun resurssin [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). Tämä ominaisuus on erityisen häiriötekijöiden siirtyminen monitasoisissa sisäkkäinen resurssit.

Lisää toimintoja ja ominaisuusarvoihin myös näkyvät Azure resurssien mukaan. Esimerkiksi web Apps-sovellusten ja logiikka sovellukset myös on **avoinna selaimessa** ja **Liitä virheenkorjaus** lisäksi **Avaa portaalissa**toiminnot. Avaa editors tulevat näkyviin, kun valitset tallennustilan tilin blob, jonossa tai taulukko. Azure sovellukset on **URL-osoite** ja **tila** ominaisuuksia, kun tallennustilan resursseja on avain ja yhteyden merkkijono-ominaisuudet.

## <a name="search-resources"></a>Etsi resursseja

Voit etsiä tietyn nimellä resurssien Azure-tili-tilauksessa, kirjoita hakuruutuun Cloud Explorerissa nimi.

![Resurssien etsiminen Cloud Resurssienhallinnassa](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC820394.png)

Kun kirjoitat merkkejä hakuruutuun, vain resurssit, jotka vastaavat merkit näkyvät resurssin puun.

