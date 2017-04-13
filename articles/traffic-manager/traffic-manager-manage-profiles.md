<properties
    pageTitle="Hallitse Azure liikenteen hallinta-profiileista | Microsoft Azure"
    description="Tämän artikkelin avulla voit luominen, poistaminen käytöstä, ota käyttöön, poistaminen ja Azure liikenteen hallinta profiilin muutoshistorian."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="manage-an-azure-traffic-manager-profile"></a>Azure liikenteen hallinta-profiilin hallinta

Liikenteen hallinta-profiileista hallita liikenteen pilvipalveluihin tai sivuston päätepisteet liikenne reititys menetelmiä avulla. Tässä artikkelissa kerrotaan, miten voit luoda ja hallita profiilit.

## <a name="create-a-traffic-manager-profile-using-quick-create"></a>Nopea luominen käyttämällä liikenteen hallinta profiilin luominen

Voit nopeasti luoda liikenteen hallinta profiilin käyttämällä Azure perinteinen portaalissa nopea luominen. Nopea luominen avulla voit luoda profiileja peruskokoonpano asetuksilla. Kuitenkaan voi käyttää nopea luominen asetuksia, kuten päätepisteet (pilvipalveluihin ja sivustoja), automaattisesti tilauksen automaattisesti liikenne reititys-menetelmää tai valvonta-asetusten määrittäminen. Kun olet luonut profiilin, voit määrittää nämä asetukset Azure perinteinen-portaalissa. Liikenteen hallinta tukee enintään 200 päätepisteet profiilissa. Useimmat käyttötapoja edellyttävät kuitenkin vain muutaman päätepisteet.

### <a name="to-create-a-traffic-manager-profile"></a>Liikenteen hallinta profiilin luominen

1. **Pilvipalveluihin ja sivustojen käyttöönotto tuotannon-ympäristöön.** Saat lisätietoja pilvipalveluihin [Pilvipalveluihin](http://go.microsoft.com/fwlink/p/?LinkId=314074). Lisätietoja sivustojen nähdä [sivustot](http://go.microsoft.com/fwlink/p/?LinkId=393327).

2. **Kirjaudu sisään Azure perinteinen-portaaliin.** **Uusi** -portaalin vasemmassa alakulmassa, valitse **verkkopalveluita > liikenteen hallinta**, ja valitse sitten Aloita profiilin määrittämisestä **Nopea luominen** .
3. **Määritä DNS-etuliite.** Anna liikenteen hallinta profiilin yksilöllinen DNS etuliite. Voit määrittää vain etuliite liikenteen hallinta toimialuenimi.
4. **Valitse tilaus.** Valitse haluamasi Azure tilaus. Kunkin profiilin on liitetty yhden tilauksen. Jos käytössäsi on vain yksi tilaus, tämä asetus ei näy.
5. **Valitse liikenne reititys-menetelmää.** Valitse liikenne reititys menetelmä **liikenne reititys käytännön**. Lisätietoja liikenne reititys menetelmiä artikkelissa [liikenteen hallinta liikenne reititys tavoista](traffic-manager-routing-methods.md).
6. **Napsauttamalla "Luo" profiilin**. Kun profiili-määritys on valmis, voit etsiä profiilin Azure perinteinen portaalissa liikenteen hallinta-ruudussa.
7. **Määritä päätepisteet, seuranta ja lisäasetuksia Azure perinteinen-portaalissa.** Perusasetukset määrittää vain käyttämällä nopea luominen. On tarpeen, voit määrittää lisäasetuksia, kuten päätepisteet ja päätepisteen automaattisesti järjestyksessä.


## <a name="disable-enable-or-delete-a-profile"></a>Poista käytöstä, ottaminen käyttöön tai poistaa profiili

Voit poistaa aiemmin luotuun profiiliin niin, että liikenteen hallinta ei viittaa pyynnöt määritetyn päätepisteet. Kun poistat liikenteen hallinta-profiili, profiilin ja profiilin sisältämät tiedot säilyvät ja voi muokata liikenteen hallinta käyttöliittymässä.  Viitteitä jatkaa, kun otat käyttöön profiili. Kun luot liikenteen hallinta profiilin Azure perinteinen-portaalissa, se on automaattisesti käytössä. Jos päätät profiilia ei enää tarvita, voit poistaa sen.

### <a name="to-disable-a-profile"></a>Profiilin poistaminen käytöstä

1. Jos käytät mukautettua toimialuenimeä, muuta CNAME-tietue Internet DNS-palvelimeen, niin, että se osoittaa enää liikenteen hallinta profiilin.
2. Liikenne lopettaa parhaillaan ohjataan päätepisteet profiilin liikenteen hallinta-asetusten avulla.
3. Valitse profiili, jonka haluat poistaa käytöstä. Liikenteen hallinta-sivulla Korosta profiilin, valitsemalla profiilinimi viereiseen sarakkeeseen. Huomautus, valitsemalla nimen ja profiilin nimen vieressä olevaa nuolta Avaa asetukset-sivun profiilin.
4. Kun olet valinnut profiilin, valitsemalla sivun alareunassa **Poista käytöstä** .

### <a name="to-enable-a-profile"></a>Jos haluat ottaa käyttöön profiili

1. Valitse profiili, jonka haluat poistaa käytöstä. Liikenteen hallinta-sivulla Korosta profiilin, valitsemalla profiilinimi viereiseen sarakkeeseen. Huomautus, valitsemalla nimen ja profiilin nimen vieressä olevaa nuolta Avaa asetukset-sivun profiilin.
2. Kun olet valinnut profiilin, valitse sivun alareunassa **käyttöön** .
3. Jos käytät mukautettua toimialuenimeä, Luo resurssin CNAME-tietue osoittamaan liikenteen hallinta profiilin toimialuenimi Internet DNS-palvelimeen.
4. Liikenne ohjataan päätepisteet uudelleen.

### <a name="to-delete-a-profile"></a>Jos haluat poistaa profiili

1. Varmista, että DNS-resurssitietue Internet DNS-palvelimeen ei enää käyttää CNAME-resurssitietue, joka osoittaa toimialueen liikenteen hallinta profiilin nimi.
2. Valitse profiili, jonka haluat poistaa käytöstä. Liikenteen hallinta-sivulla Korosta profiilin, valitsemalla profiilinimi viereiseen sarakkeeseen. Huomautus, valitsemalla nimen ja profiilin nimen vieressä olevaa nuolta Avaa asetukset-sivun profiilin.
3. Kun olet valinnut profiilin, valitse sivun alareunassa **Poista** .

## <a name="view-traffic-manager-profile-change-history"></a>Näkymien liikenteen hallinta profiilin muutosloki

Voit tarkastella muutoslokin liikenteen hallinta profiilin Azure perinteinen portaalissa Management Services-palveluissa.

### <a name="to-view-your-traffic-manager-change-history"></a>Jos haluat tarkastella muutoslokin liikenteen hallinta

1. Valitse **Hallintapalvelut**Azure perinteinen portaalin vasemmanpuoleisessa ruudussa.
2. Valitse Management Services-sivulla **Toimintojen lokit**.
3. Toiminnon lokit-sivulla voit suodattaa liikenteen hallinta profiilin muutoshistorian tarkasteleminen. Kun olet valinnut suodatuksen asetuksia, napsauta tulokset valintamerkkiä.

   - Voit tarkastella kaikkien profiilien muutoksia, valitse tilaus ja aikavälin ja valitse sitten pikavalikosta **tyyppi** **Liikenteen hallinta** .
   - Voit suodattaa profiilinimi, kirjoita profiilin nimi **Palvelunimi** -kenttään tai valitse se pikavalikosta.
   - Voit tarkastella tietoja yksittäisen muutoksen, valitse kyseinen rivi, jonka haluat tarkastella muutoksen ja valitse sitten **tiedot** sivun alareunassa. Valitse **Toiminnon tiedot** -ikkunassa voit tarkastella API-objekti, joka on luotu tai päivitetään osana toiminnon XML-esitys.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Lisää päätepisteen](traffic-manager-endpoints.md)
- [Määritä automaattisesti reititys menetelmä](traffic-manager-configure-failover-routing-method.md)
- [PYÖRISTÄ-funktiota Mikko reititys menetelmä määrittäminen](traffic-manager-configure-round-robin-routing-method.md)
- [Määritä suorituskyvyn reititys menetelmä](traffic-manager-configure-performance-routing-method.md)
- [Yrityksen Internet-toimialueen osoittamista liikenteen hallinta toimialuenimi](traffic-manager-point-internet-domain.md)
- [Vianmäärityksen liikenteen hallinta heikentynyt tila](traffic-manager-troubleshooting-degraded.md)