<properties
    pageTitle="Azure App palvelun suunnitelmien perusteellisempaa yleiskatsaus | Microsoft Azure"
    description="Katso, miten sovelluksen palvelun suunnitelmien Azure App Service-työstä, miten hyötyä käyttökokemuksen hallinta."
    keywords="sovelluksen, azure app palvelun, skaalaus-skaalattava-sovelluksen palvelusopimus-sovelluksen palvelun kustannukset"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="byvinyal"/>

# <a name="azure-app-service-plans-in-depth-overview"></a>Azure App palvelun suunnitelmien perusteellisempaa yleiskatsaus#

Sovelluksen-palvelusopimus on tietyt ominaisuudet ja kapasiteettia, voit jakaa useita sovelluksia. Web Apps-sovelluksista, Mobile-sovellusten, funktio sovellukset tai API-sovellusten [Azure App palvelun](http://go.microsoft.com/fwlink/?LinkId=529714) kaikki toimivat sovelluksen palvelusopimus. Näistä palvelupaketeista tukevat viisi hinnoittelu tasoa: *vapaa*, *jaettu*, *Basic*, *Vakio*ja *Premium*. Kunkin tason on omia ominaisuuksia ja kapasiteetti. Sovellusten maantieteellisen sijainnin ja saman tilauksen jakaa suunnitelma. Jakaminen-suunnitelman olevia sovelluksia voit käyttää kaikkia ominaisuuksia ja toimintoja, jotka on määritetty suunnitelman tason mukaan. Kaikki sovellukset, jotka liittyvät suunnitelma suorittaa resurssit, joka määrittää suunnitelma.

Esimerkiksi jos palvelupakettisi on määritetty käyttämään kaksi "pieni" esiintymät vakiorivejä taso, kaikki sovellukset, jotka liittyvät suunnitelman suorittaa molemmat esiintymät ja käyttämään vakiorivejä taso-toimintoja. Suunnitelman esiintymät, jossa apps on käynnissä on täysin hallittuja ja erittäin käytettävissä.

Tässä artikkelissa käsitellään tärkeimmät ominaisuudet, kuten taso ja skaalaus-sovelluksen-palvelusopimus ja kuinka ne tulevat kyselyjä toista samalla kun hallitset sovelluksia.

## <a name="apps-and-app-service-plans"></a>Sovellusten ja sovelluksen palvelusopimusten vaihtoehdot

Sovelluksen Service-sovelluksen voi olla vain yksi sovellus-palvelusopimus on liitetty.

Sovellusten ja suunnitelmien sisältyvät resurssiryhmä. Resurssiryhmä toimii kunkin resurssin se sisältyvä elinkaari-reunaa. Resurssiryhmien avulla voit hallita kaikki sovelluksen osat yhdessä.

Koska yksi resurssiryhmä voi olla useita sovelluksen palvelusopimusten vaihtoehdot, voit kohdistaa eri sovellusten eri fyysinen resursseihin. Voit esimerkiksi erottaa resursseja keskihajonta, Testaa ja tuotannon ympäristöissä kesken. Ottaa erillisestä tuotannon ja keskihajonta/testin avulla voit paikantaa resurssit. Tällä tavalla kuormituksen testaaminen vastaan uuden version sovellus kilpailemaan saman resurssien kuin tuotannon sovelluksia, jotka ovat käsittelevä reaali asiakkaille.

Kun sinulla on useita suunnitelmien yksittäisen resurssiryhmä, voit myös määrittää sovellus, joka ulottuu maantieteellisillä alueilla. Esimerkiksi erittäin käytettävissä sovellus, joka on käynnissä kummallakin alueella on vähintään kaksi suunnitelmat, yhteen kunkin alueen ja yksi kullekin palvelupakettiin liitetyn sovelluksen. Tällaisessa sovelluksen kopioita sisältyvät sitten yksi resurssiryhmä. On useita suunnitelmien ja useiden sovelluksista resurssiryhmä helpottaa hallintaa, hallita ja tarkastella sovelluksen kunto.

## <a name="create-an-app-service-plan-or-use-existing-one"></a>Luo sovellus-palvelusopimus tai Käytä aiemmin luotuun

Kun luot sovelluksen, ota huomioon Resurssiryhmän luominen. Toisaalta, jos sovellus, jota olet luomassa on suurempi sovelluksen osa, sovelluksen luodaanko resurssiryhmän, joka on varattu suurempi sovelluksen sisällä.

Onko uusi sovellus on kokonaan uuden sovelluksen tai suurempi yksi osa, voit isännöidä sitä tai luoda uuden aiemmin App palvelusopimus avulla. Tämä päätös on Lisää kysymys kapasiteetin ja odotettua.

Jos on olemassa olevan järjestelyn käyttöön eri tekijät muista sovelluksista skaalaus isännöidään uuden sovelluksen suorittaminen käyttää paljon resursseja, on suositeltavaa, että se eristää omassa palvelupakettiin.

Kun luot suunnitelma, voit uudella valikoimalla resurssien varaaminen sovelluksen ja saada resurssivaraukset tarkemmin, koska kunkin suunnitelman saa omassa esiintymiä.

Koska voit siirtää sovellukset-palvelupaketeissa, voit muuttaa tapaa, jolla resurssit varataan isommaksi sovelluksen kautta.

Lopuksi, jos haluat luoda sovelluksen toisella alueella ja että alue ei ole olemassa olevan järjestelyn, suunnittele alueella olevat voivat ylläpitää sovellukseen tässä.

## <a name="create-an-app-service-plan"></a>Luo sovellus-palvelusopimus

>[AZURE.TIP] Jos sinulla on sovelluksen Service-ympäristössä voit tarkastella niitä tietyn sovelluksen palvelun ympäristöt tähän: [Luo-sovelluksen palvelun suunnitteleminen App Service-ympäristössä](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)

Voit luoda tyhjän sovelluksen palvelusopimus App palvelun suunnitelman Selaa kokemus tai sovelluksen luomisen osana.

[Azure-portaali](https://portal.azure.com)valitsemalla **Uusi** > **Web + mobile**, ja valitse sitten **Web App** tai sovelluksen palvelun sovelluksen muita kuvia.
![Sovelluksen luominen Azure-portaalissa.][createWebApp]

Voit valita tai luoda uuden sovelluksen App palvelusopimus.

 ![Luo sovellus-palvelusopimus.][createASP]

Jos haluat luoda uuden sovelluksen palvelun suunnitelman, napsauttamalla **+ Luo uusi**, **sovelluksen palvelusopimus** nimi ja valitse sitten oikeaan **paikkaan**. Valitse **hinnoittelu taso**ja valitse sitten haluamasi hinnoittelu taso-palvelun. Valitse Näytä Lisää hinnoittelu asetukset, kuten **vapaa** ja **jaettujen** **Näytä kaikki** . Kun olet valinnut hinnoittelu tason, napsauta **Valitse** -painiketta.

## <a name="move-an-app-to-a-different-app-service-plan"></a>Siirtää sovelluksen eri App palvelusopimus

Voit siirtää eri sovelluksen-palvelusopimus [Azure portal](https://portal.azure.com)-sovellus. Voit siirtää sovellusten palvelupakettien välillä, kunhan tilauksia samassa resurssiryhmä ja maantieteellinen alue.

Jos haluat siirtää toiseen sovelluksen, siirry sovellukseen, jonka haluat siirtää. **Asetukset** -valikosta Etsi **Sovellus palvelun suunnittelu**.

**Muuta sovelluksen palvelusopimus** avautuu **sovelluksen palvelusopimus** -valitsin. Tässä vaiheessa voit valita olemassa olevan järjestelyn tai luoda uuden. Vain kelvollisia suunnitelmien (käyttöä saman resurssiryhmä ja maantieteellisen sijainnin) ovat näkyvissä.

![Sovelluksen palvelun suunnitelman valitseminen.][change]

Kunkin suunnitelmassa on omaa hinnoittelua taso. Esimerkiksi kun siirrät sivuston vapaa taso, vakio taso-sovelluksen nyt käyttää kaikkia ominaisuuksia ja resurssien vakio tason.

## <a name="clone-an-app-to-a-different-app-service-plan"></a>Kloonaa sovelluksen eri App palvelusopimus
Jos haluat siirtää sovelluksen toisella alueella, yksi vaihtoehto on sovelluksen kloonaamalla. Kloonaamalla tekee kopion sovelluksen uuteen tai aiemmin luotuun App palvelusopimus tai sovelluksen Service-ympäristössä, minkä tahansa alueella.

 ![Kloonaa sovelluksen.][appclone]

Löydät **Kloonaa sovelluksen** **Työkalut** -valikosta.

Kloonaamalla on joitakin rajoituksia, jotka on lisätietoja [Azure App palvelun sovelluksen kloonaamalla Azure-portaalissa](../app-service-web/app-service-web-app-cloning-portal.md)osoitteessa.

## <a name="scale-an-app-service-plan"></a>Skaalaa sovelluksen-palvelusopimus

Jos haluat skaalata suunnitelma kolmella eri tavalla:

- **Muuta suunnitelman hinnat taso**. Esimerkiksi Basic tason suunnitelma voidaan muuntaa Standard tai Premium taso ja kaikki sovellukset, jotka liittyvät suunnitelman nyt käyttää ominaisuuksia, jotka on uusi palvelutaso.
- **Suunnitelman esiintymän koon muuttaminen**. Esimerkkinä Basic taso, joka käyttää pieni esiintymät suunnitelma voi muuttaa käyttämään suuri esiintymät. Kaikki sovellukset, jotka liittyvät suunnitelman nyt käyttää muita muistin ja suorittimen resursseja, joka tarjoaa esiintymän suurempia.
- **Suunnitelman esiintymän määrän muuttaminen**. Esimerkiksi vakio suunnitelma, joka on skaalattu kolme esiintymissä skaalata 10 esiintymissä. Premium-palvelupaketin voi skaalata 20 esiintymissä (veloittaa saatavuuden). Kaikki sovellukset, jotka liittyvät suunnitelman nyt käyttää muita muistin ja suorittimen resursseja, joka tarjoaa suuremman esiintymän määrä.

Voit muuttaa hinnoittelu taso ja esiintymän koko sovelluksen tai palvelun sovelluksen asetukset-kohdassa **Asteikko ylös** -kohteen. Muutokset koskevat sovelluksen palvelusopimus ja vaikuttavat kaikki sovellukset, jotka nimeksi isännät.

 ![Määrittää arvot, jos haluat skaalata sovelluksen ylöspäin.][pricingtier]

## <a name="app-service-plan-cleanup"></a>Sovelluksen palvelun suunnitteleminen uudelleenjärjestäminen
**Sovelluksen palvelusopimusten vaihtoehdot** , joilla ei ole niihin liittyvät sovellukset maksamaan ovat edelleen, koska ne edelleen varata Laske kapasiteettia, määritetty sovelluksen palvelun suunnitelman asteikko-ominaisuudet.
Voit välttää odottamattomia kulujen ylläpidettävä App palvelusopimus viimeisen sovellus poistetaan, tuloksena oleva tyhjä App palvelun suunnitelman poistetaan myös.


## <a name="summary"></a>Yhteenveto

Sovelluksen palvelusopimusten vaihtoehdot edustavat tietyt ominaisuudet ja kapasiteettia, voit jakaa sovelluksia. Sovelluksen palvelusopimusten vaihtoehdot avulla voit kohdistaa resurssien joukon tietyn sovelluksia ja optimoida edelleen Azure resurssien käyttö joustavasti. Tällä tavalla, jos haluat tallentaa rahaa testauksen ympäristössäsi voit jakaa suunnitelma useita sovelluksia. Voit suurentaa siirtonopeuden tuotannon ympäristössä myös skaalaus eri alueilla ja suunnitelmien välillä.

## <a name="whats-changed"></a>Mikä on muuttunut

* Muuta opas verkkosivuilta sovelluksen-palveluun, saat: [Azure App palvelu ja sen vaikutus aiemmin Azure-palvelut](http://go.microsoft.com/fwlink/?LinkId=529714)

[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
[appclone]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/app-clone.png
