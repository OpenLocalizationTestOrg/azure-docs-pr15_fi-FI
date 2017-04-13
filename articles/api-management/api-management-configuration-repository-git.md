<properties 
    pageTitle="Tallenna ja käyttämällä Git API hallinta palvelun kokoonpanon määrittäminen" 
    description="Lue, miten voit tallentaa ja käyttämällä Git API hallinta palvelun kokoonpanon määrittäminen." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>


# <a name="how-to-save-and-configure-your-api-management-service-configuration-using-git"></a>Tallenna ja käyttämällä Git API hallinta palvelun kokoonpanon määrittäminen

>[AZURE.IMPORTANT] Git määritys API hallintaa varten on tällä hetkellä esikatselu. Toiminnallisesti päätyttyä, mutta on esikatselussa, koska olemme aktiivisesti alkamiskohdan palautetta tämän toiminnon. On mahdollista, että emme voi tehdä jakautumisen, muuttaa asiakaspalautteen, jotta Suosittelemme ei sen mukaan, käytä tuotantoympäristössä ominaisuutta. Jos sinulla on palautetta tai kysymyksiä, kerro meille `apimgmt@microsoft.com`.

Kunkin API Management-palvelun esiintymän ylläpitää kokoonpanotietokannan, joka sisältää tietoja määritys ja palveluesiintymä-metatiedot. Publisher-portaalissa asetuksen muuttaminen, PowerShell-cmdlet-komennolla tai puhelun soittaminen REST API voit tehdä muutoksia palveluesiintymä. Näistä tavoista lisäksi voit hallita service-esiintymän määrittäminen käyttämällä Git, ottaminen service management skenaariot kuten:

-   Määritysten versiotiedot - ladata ja tallentaa eri versioita service-määritys
-   Joukkosähköposti määritysmuutoksia - muutosten tekeminen useita osia service-määritys paikallisen säilössä ja muutokset palvelimeen integrointi yhdellä kertaa
-   Tutut Git toolchain ja työnkulku - Git sillä ja työnkulkuja, jotka olet jo aiemmin käyttäminen

Seuraavassa kaaviossa on esitetty yleiskatsaus määrittäminen API Management-palvelun esiintymän eri tavalla.

![Git määrittäminen][api-management-git-configure]

Kun olet tehnyt muutokset palvelu publisher-portaalissa, PowerShellin cmdlet-komennot tai REST-Ohjelmointirajapinta, hallinnoit, että palvelun määritys-tietokannan käyttäminen `https://{name}.management.azure-api.net` päätepisteen, kuten kaavion oikeassa reunassa. Kaavion vasemmalla puolella on kuvattu siitä, miten voit hallita palvelun asetusten käyttäminen Git ja Git säilö palveluun osoitteessa `https://{name}.scm.azure-api.net`.

Seuraavat vaiheet sisältävät yleisiä oman API hallinta esiintymää käyttämällä Git hallinta.

1.  Palvelun Git-käytön käyttöön ottaminen
2.  Tallenna palvelun kokoonpanotietokannan Git säilöön
3.  Kloonaa Git repo paikalliseen tietokoneeseen
4.  Tuoda alaspäin paikallisessa tietokoneessa uusin repo ja Vahvista ja push muutokset takaisin oman repo
5.  Oman repo muutokset käyttöön palvelun määritys-tietokantaan

Tässä artikkelissa kuvataan ottaminen käyttöön ja Git avulla voit hallita palvelun kokoonpanon ja ohjeet tiedostojen ja kansioiden Git säilössä.

## <a name="to-enable-git-access"></a>Jotta voit käyttää Git

Voit tarkastella Git kokoonpanosi tilan nopeasti tarkastelemalla Git kuvaketta oikeassa alakulmassa publisher-portaalissa. Tässä esimerkissä Git access ei vielä ole käytössä.

![Git tila][api-management-git-icon-enable]

Voit tarkastella ja että Git asetuksia, joko Git-kuvaketta tai valitse **Suojaus** -valikko ja siirry **määritysten säilöön** -välilehti.

![Ota käyttöön GIT][api-management-enable-git]

Git käytön käyttöön **ottaminen käyttöön Git access** -valintaruutu.

Hetken kuluttua muutos on tallennettu ja vahvistusviesti tulee näkyviin. Huomaa, että Git kuvake muuttuu väri, joka ilmaisee, että Git käyttö on käytössä ja tilasanoma osoittaa nyt, joka on tallentamattomia muutoksia säilöön. Tämä johtuu siitä API Management-palvelun kokoonpanotietokannan ei on vielä tallennettu säilöön.

![Git käytössä][api-management-git-enabled]

>[AZURE.IMPORTANT] Tietoja, joita ei ole määritetty ominaisuudet tallennetaan säilössä, ja ne ovat sen historiatietoihin vasta, kun käytöstä ja ottaminen käyttöön Git access. Ominaisuudet sisältävät hallittavan vakion merkkijonoarvot, kuten tietoja, kaikki API määritys ja käytännöistä, joten sinun ei tarvitse tallentaa ne suoraan käytännön lauseita turvallisessa paikassa. Lisätietoja on artikkelissa [Azure API hallintakäytännöt ominaisuuksien käyttämisestä](api-management-howto-properties.md).

Artikkelissa tietojen käyttöönoton tai käytöstäpoiston Git käytön REST-Ohjelmointirajapinta, [ottaminen käyttöön tai poistaminen käytöstä Git käyttöoikeuksien REST-Ohjelmointirajapinnan käyttäminen](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).

## <a name="to-save-the-service-configuration-to-the-git-repository"></a>Jos haluat tallentaa palvelun Git säilöön

Ennen kloonaamalla säilö ensimmäinen vaihe on tallentaa palvelun nykyisen tilan säilö. Valitse **Tallenna määritysten säilöön**.

![Tallenna määritys][api-management-save-configuration]

Tee haluamasi muutokset vahvistusnäytössä ja valitse **Ok** voit tallentaa.

![Tallenna määritys][api-management-save-configuration-confirm]

Hetken kuluttua määritykset tallennetaan ja tietovaraston määritys-tila näkyy, kuten päivämäärän ja ajan määrittäminen viimeisin muutos ja edellisen palvelun ja säilö välisen synkronoinnin.

![Tilan määrittäminen][api-management-configuration-status]

Kun määritykset on tallennettu säilöön, se voi kopioida.

Saat lisätietoja tämän toiminnon avulla REST API-artikkelissa [Vahvista määritysten tilannevedoksen REST-Ohjelmointirajapinnan käyttäminen](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).

## <a name="to-clone-the-repository-to-your-local-machine"></a>Jos haluat Kloonaa säilö, johon paikallisessa tietokoneessa

Kloonaa säilö, tarvitset URL-Osoitteen lisääminen säilöön, käyttäjänimen ja salasanan. Käyttäjänimi ja URL-osoite näytetään **määritysten säilöön** -välilehden yläosassa.

![Git Kloonaa][api-management-configuration-git-clone]

Salasana luodaan **määritysten säilöön** -välilehden alareunassa.

![Luo salasana][api-management-generate-password]

Voit luoda salasanan, ensin varmistamaan, että **määräajan** on haluamasi vanhentumispäivämäärä ja aika ja valitse sitten **Luo tunnuksen**.

![Salasana][api-management-password]

>[AZURE.IMPORTANT] Pane merkille salasana. Kun poistut tältä sivulta salasanaa ei näytetä uudelleen.

Seuraavissa esimerkeissä [Git for](http://www.git-scm.com/downloads) Windowsista Git Bash-työkalun avulla, mutta voit käyttää mitä tahansa Git-työkalua, jotka ovat sinulle tuttuja.

Git-työkalun avaaminen haluamasi kansio ja suorita seuraava komento Kloonaa git-säilön paikallisessa tietokoneessa, publisher-portaalin myöntämä-komennolla.

    git clone https://bugbashdev4.scm.azure-api.net/ 

Anna käyttäjänimi ja salasana pyydettäessä.

Jos näyttöön tulee virheitä, yritä muokkaaminen oman `git clone` komento lisää käyttäjänimi ja salasana, kuten seuraavassa esimerkissä.

    git clone https://username:password@bugbashdev4.scm.azure-api.net/

Jos tämä on virheen, yritä URL-Osoitteen koodaus-komennon salasana-osaan. Yksi nopeasti toiminto on avata Visual Studio ja antaa **Välittömän**seuraava komento. Avaa **Välittömän suoritusruudun**Visual Studio-ratkaisusta tai -projektin avaaminen (tai Luo uusi tyhjä console-sovellus) ja valitse sitten **Windows**- **Välitön** **Virheenkorjaus** -valikossa.

    ?System.NetWebUtility.UrlEncode("password from publisher portal")

Käyttäjän nimi ja säilön sijainti sekä koodattu salasanan avulla voit käyttää git-komento.

    git clone https://username:url encoded password@bugbashdev4.scm.azure-api.net/

Kun säilö on kopioitu, voit tarkastella ja käyttää sitä paikallisessa tiedostojärjestelmässä. Lisätietoja on artikkelissa [tiedostojen ja kansioiden rakenne viittaus paikallisen Git tietovaraston](#file-and-folder-structure-reference-of-local-git-repository).

## <a name="to-update-your-local-repository-with-the-most-current-service-instance-configuration"></a>Voit päivittää paikallisen säilöön uusimmat service esiintymä-määritys

Jos teet muutoksia API Management service-esiintymässä publisher-portaalin tai käyttämällä REST-Ohjelmointirajapinta, on tallennettava muutokset säilöön, ennen kuin voit päivittää paikallisen säilöön uusimmat muutokset. Voit tehdä tämän valitsemalla publisher-portaalin **määritysten säilöön** -välilehden **Tallenna määritysten säilöön** ja antaa seuraava komento paikalliseen säilöön.

    git pull

Ennen sen suorittamista `git pull` Varmista, että olet kansion paikallisen säilöön varten. Jos olet juuri suorittanut `git clone` komennon, sinun on muutettava hakemiston oman repo suorittamalla seuraavalta komennon.

    cd bugbashdev4.scm.azure-api.net/

## <a name="to-push-changes-from-your-local-repo-to-the-server-repo"></a>Viemään paikallisen repo muutokset palvelimeen repo

Painaa muutokset paikalliseen säilöön palvelimen säilö tekemäsi muutokset ja push ne palvelimen säilöön. Voit tallentaa muutokset Git komento-työkalun avaaminen, siirry hakemiston paikallisen säilön ja annettava seuraavat komennot.

    git add --all
    git commit -m "Description of your changes"

Jos haluat siirtää kaikki tapahtumasarjan vahvistukset palvelimeen, suorita seuraava komento.

    git push

## <a name="to-deploy-any-service-configuration-changes-to-the-api-management-service-instance"></a>Palvelun määritysten muutokset käyttöön API Management-palveluesiintymä

Kun paikalliset muutokset on vahvistettu ja miten palvelimen säilöön, voit ottaa ne API Management-palveluesiintymä.

![Ottaa käyttöön][api-management-configuration-deploy]

Saat lisätietoja tämän toiminnon avulla REST-Ohjelmointirajapinta tapahtuu [REST-Ohjelmointirajapinnan käyttäminen kokoonpanotietokannan käyttöönotto Git muutoksia](https://msdn.microsoft.com/library/dn781420.aspx#DeployChanges).

## <a name="file-and-folder-structure-reference-of-local-git-repository"></a>Tiedostojen ja kansioiden rakenne viittaus paikallisen Git säilöön

Tiedostojen ja kansioiden paikallisen git säilössä sisältää palvelun esiintymän kokoonpanotietoja.

| Kohteen                       | Kuvaus                                                                                |
|-------------------------   |--------------------------------------------------------------------------------------------|
| pääkansio api hallinta | Sisältää palvelun esiintymän ylimmän tason määritys                                  |
| API-kansio                | Sisältää palvelun esiintymän ohjelmointirajapinnan määrityskohde                            |
| Ryhmät-kansiossa              | Sisältää ryhmien service-esiintymän määrittäminen                          |
| käytännöt kansio            | Sisältää palvelun esiintymän käytännöt                                              |
| portalStyles-kansio        | Sisältää developer portaalin mukautusten service-esiintymän määrittäminen |
| tuotteet-kansio            | Sisältää tuotteiden service-esiintymän määrittäminen                        |
| Mallit-kansioon           | Sisältää palvelun esiintymän sähköpostimalleja määrityskohde                 |

Kullakin kansiolla voi olla vähintään yksi tiedosto ja joissakin tapauksissa yhdestä tai useammasta kansiosta, esimerkiksi kansion kunkin API, tuotteen tai ryhmän. Kunkin tiedostot ovat tietyn kansionimi on kuvattu kohteen tyyppi.

| Tiedostotyyppi | Tarkoitus                                                                |
|-----------|------------------------------------------------------------------------|
| JSON      | Kunkin kohteen kokoonpanotietoja                  |
| HTML      | Tietoja kohteen tulevat yleensä näkyviin developer-portaalissa kuvaukset |
| XML       | Käytännön lauseet                                                      |
| CSS-koodin       | Kehitystyökalut-portaalin mukauttaminen tyylisivut                        |

Nämä tiedostot voidaan luoda, poistaa, muokata, ja hallita paikallisen tiedostojärjestelmän ja ottaa käyttöön muutosten Ohjelmointirajapinnan Management-palveluesiintymä.

>[AZURE.NOTE] Seuraavia kohteita ei sisälly Git säilö ja ei voi määrittää Git.
>
>-    Käyttäjät
>-    Tilaukset
>-    Ominaisuudet:
>-    Kehittäjän portaalin kohteet kuin tyylit

### <a name="root-api-management-folder"></a>Pääkansio api hallinta

Pääkansio `api-management` kansion sisältö `configuration.json` palvelun esiintymän seuraavanlaisen ylätason tietoja sisältävän tiedoston.

    {
      "settings": {
        "RegistrationEnabled": "True",
        "UserRegistrationTerms": null,
        "UserRegistrationTermsEnabled": "False",
        "UserRegistrationTermsConsentRequired": "False",
        "DelegationEnabled": "False",
        "DelegationUrl": "",
        "DelegatedSubscriptionEnabled": "False",
        "DelegationValidationKey": ""
      },
      "$ref-policy": "api-management/policies/global.xml"
    }

Neljä ensimmäistä asetukset (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, ja `UserRegistrationTermsConsentRequired`) Yhdistä **käyttäjätiedot** -välilehden **Suojaus** -kohdassa seuraavat asetukset.

| Tunnistetiedot-asetus                     | Yhdistää                                               |
|--------------------------------------|-------------------------------------------------------|
| RegistrationEnabled                  | **Ohjaa kirjautumissivu anonyymin** valintaruutu |
| UserRegistrationTerms                | **Valitse käyttäjän kirjautuminen käyttöehdot** tekstiruutu               |
| UserRegistrationTermsEnabled         | **Näytä käyttöehdot kirjautumissivulla** valintaruutu         |
| UserRegistrationTermsConsentRequired | **Edellyttävät suostumuksen** valintaruutu                          |

![Käyttäjätietojen asetukset][api-management-identity-settings]

Seuraavat neljä asetukset (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, ja `DelegationValidationKey`) Yhdistä **delegointi** -välilehden **Suojaus** -kohdassa seuraavat asetukset.

| Delegointi-asetus           | Yhdistää                                    |
|------------------------------|--------------------------------------------|
| DelegationEnabled            | **Edustajan Kirjaudu sisään ja ilmoittautuminen** -valintaruutu    |
| DelegationUrl                | **Delegointi päätepisteen URL-osoite** -tekstiruutu        |
| DelegatedSubscriptionEnabled | **Edustajan tuotteen tilauksen** -valintaruutu |
| DelegationValidationKey      | **Edustajan vahvistus avain** tekstiruutu        |

![Delegoinnin asetukset][api-management-delegation-settings]

Lopullinen asetus `$ref-policy`, maps-palvelun esiintymän yleistä käytäntöä lauseet-tiedostoon.

### <a name="apis-folder"></a>API-kansio

`apis` Kansio sisältää kansion kunkin API palvelun esiintymän, joka sisältää seuraavat kohteet.

-   `apis\<api name>\configuration.json`– Tämä on Ohjelmointirajapinnan määrityskohde ja sisältää tietoja Taustajärjestelmä palvelun URL-osoite ja toiminnot. Tämä on samat tiedot palautetaan, jos kutsu avulla [Saat tietyn API](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) `export=true` - `application/json` muodossa.
-   `apis\<api name>\api.description.html`– Tämä on Ohjelmointirajapinnan kuvaus ja vastaa `description` [API kohteen](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties)ominaisuus.
-   `apis\<api name>\operations\`– Tämä kansio sisältää `<operation name>.description.html` tiedostot, jotka vastaavat Ohjelmointirajapinnan toimintoihin. Kunkin tiedosto sisältää yhdellä kertaa, joka yhdistää API-kuvaus `description` ominaisuuden REST-Ohjelmointirajapinnalla [toiminnon kohteen](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) .

### <a name="groups-folder"></a>Ryhmät-kansiossa

`groups` Kansio sisältää kunkin ryhmän määritetty palvelun esiintymän kansion.

-   `groups\<group name>\configuration.json`– Tämä on ryhmän määritys. Tämä on palautetaan Soita [tietyn ryhmän Hae](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) -toiminto on samat tiedot.
-   `groups\<group name>\description.html`– Tämä on ryhmän kuvaus ja vastaa `description` ominaisuuden [ryhmän kohteen](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties).

### <a name="policies-folder"></a>käytännöt kansio

`policies` Kansio sisältää palvelun esiintymän käytännön lauseita.

-   `policies\global.xml`-sisältää palvelun esiintymän yleinen alueessa käytäntöjä.
-   `policies\apis\<api name>\`– Jos sinulla on määritetty API laajuus käytäntöjä, jotka kuuluvat tähän kansioon.
-   `policies\apis\<api name>\<operation name>\`-kansiossa, jos sinulla on määritetty toiminto laajuus käytäntöjä, ne sijaitsevat tämän kansion `<operation name>.xml` tiedostot, jotka vastaavat kunkin toiminnon käytännön lauseita.
-   `policies\products\`– Jos sinulla on määritetty tuotteen laajuuteen käytäntöjä, ne sijaitsevat tähän kansioon, joka sisältää `<product name>.xml` tiedostot, jotka vastaavat kunkin tuotteen käytäntö-lauseet.

### <a name="portalstyles-folder"></a>portalStyles-kansio

`portalStyles` Kansio sisältää määritys ja tyyli-taulukoiden developer portaalin mukautukset palveluesiintymä.

-   `portalStyles\configuration.json`-sisältää nimet developer-portaalissa käyttämä tyylisivut
-   `portalStyles\<style name>.css`-kunkin `<style name>.css` tiedosto sisältää tyylien developer-portaalissa (`Preview.css` ja `Production.css` oletusarvoisesti).

### <a name="products-folder"></a>tuotteet-kansio

`products` Kansio sisältää jokaisen palvelun esiintymän määritelty kansion.

-   `products\<product name>\configuration.json`– Tämä on tuotteen määritys. Tämä on palautetaan Soita [Hae tietyn tuotteen](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) -toiminto on samat tiedot.
-   `products\<product name>\product.description.html`– Tämä on tuotteen kuvaus ja vastaa `description` ominaisuuden REST-Ohjelmointirajapinnalla [tuote-kohteen](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) .

### <a name="templates"></a>mallit

`templates` Kansio sisältää palvelun esiintymän [sähköpostimalleja](api-management-howto-configure-notifications.md) määrittäminen.

-   `<template name>\configuration.json`– Tämä on sähköpostiviestin mallin määritys.
-   `<template name>\body.html`– Tämä on sähköpostimalli tekstiosaan.

## <a name="next-steps"></a>Seuraavat vaiheet

Muita tapoja hallita palvelun esiintymän tietoja on artikkelissa:

-   Hallitse oman esiintymää käyttämällä PowerShellin cmdlet-komennot
    -   [Palvelun käyttöönoton PowerShell cmdlet-viittaus](https://msdn.microsoft.com/library/azure/mt619282.aspx)
    -   [Hallinnan PowerShell cmdlet-viittaus](https://msdn.microsoft.com/library/azure/mt613507.aspx)
-   Palvelun-esiintymän publisher-portaalin hallinta
    -   [Ensimmäinen API hallinta](api-management-get-started.md)
-   Hallitse oman esiintymää REST-Ohjelmointirajapinnan käyttäminen
    -   [API hallinta REST API-viiteopas](https://msdn.microsoft.com/library/azure/dn776326.aspx)

## <a name="watch-a-video-overview"></a>Katso video yleiskatsaus

> [AZURE.VIDEO configuration-over-git]

[api-management-enable-git]: ./media/api-management-configuration-repository-git/api-management-enable-git.png
[api-management-git-enabled]: ./media/api-management-configuration-repository-git/api-management-git-enabled.png
[api-management-save-configuration]: ./media/api-management-configuration-repository-git/api-management-save-configuration.png
[api-management-save-configuration-confirm]: ./media/api-management-configuration-repository-git/api-management-save-configuration-confirm.png
[api-management-configuration-status]: ./media/api-management-configuration-repository-git/api-management-configuration-status.png
[api-management-configuration-git-clone]: ./media/api-management-configuration-repository-git/api-management-configuration-git-clone.png
[api-management-generate-password]: ./media/api-management-configuration-repository-git/api-management-generate-password.png
[api-management-password]: ./media/api-management-configuration-repository-git/api-management-password.png
[api-management-git-configure]: ./media/api-management-configuration-repository-git/api-management-git-configure.png
[api-management-configuration-deploy]: ./media/api-management-configuration-repository-git/api-management-configuration-deploy.png
[api-management-identity-settings]: ./media/api-management-configuration-repository-git/api-management-identity-settings.png
[api-management-delegation-settings]: ./media/api-management-configuration-repository-git/api-management-delegation-settings.png
[api-management-git-icon-enable]: ./media/api-management-configuration-repository-git/api-management-git-icon-enable.png




