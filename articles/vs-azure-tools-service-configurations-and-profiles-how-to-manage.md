<properties
   pageTitle="Palvelun määrityksiä ja profiilien hallinta | Microsoft Azure"
   description="Opettele palvelun määrityksiä ja profiilit määritysten tiedostoja | joka asetusten käyttöönotto-ympäristössä tallentaa ja julkaista pilvipalveluihin asetukset."
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

# <a name="how-to-manage-service-configurations-and-profiles"></a>Palvelun määrityksiä ja profiilien hallinta

## <a name="overview"></a>Yleiskatsaus

Kun julkaiset pilvipalveluun, Visual Studio tallentaa kokoonpanotietoja kahdenlaisia tiedostojen: palvelun määritykset ja profiileihin. Palvelun määrityksiä (.cscfg tiedostot) tallennetaan asetukset Azure pilvipalvelussa, käyttöönotto-ympäristössä. Azure käyttää näitä määritystiedostoja, kun se hallitsee cloud Services-palvelut. Toisaalta-profiilit (.azurePubxml tiedostot) kaupan julkaista pilvipalveluihin asetuksia. Nämä asetukset ovat kirjaa siitä, mitä valitset Julkaise ohjatulla ja Visual Studio käyttävät paikallisesti. Tässä ohjeaiheessa kerrotaan, miten molemmat tiedostotyypit, määritys-käyttöä varten.

## <a name="service-configurations"></a>Palvelun määritykset

Voit luoda useita palvelun määrityksiä käyttämään kunkin käyttöönotto-ympäristössä. Voit esimerkiksi luoda paikallisen ympäristön, jonka avulla Suorita ja testaa Azure sovelluksen ja toisen palvelun kokoonpanon tuotannon ympäristössä kohteen.

Voit lisätä, poistaa, nimetä uudelleen ja muokata palvelun määritysten tarpeen mukaan. Voit hallita palvelua määritysten Visual Studio seuraavassa kuvassa esitetyllä tavalla.

![Palvelun määrityksiä hallinta](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-service-config.png)

Voit myös avata **Hallinta määritykset** -valintaikkuna roolin ominaisuusikkunat. Avaa ominaisuudet-roolin Azure projektin-roolin pikavalikon Avaa ja valitse sitten **Ominaisuudet**. **Asetukset** -välilehden Laajenna **Service Configuration** -luettelosta ja valitse sitten **Hallitse** Avaa **Hallinta määritykset** -valintaikkuna.

### <a name="to-add-a-service-configuration"></a>Jos haluat lisätä palvelun määritykset

1. Napsauta ratkaisunhallinnassa Azure projektin pikavalikon Avaa ja valitse sitten **Hallitse määrityksiä**.

    **Hallitse palvelun määritykset** -valintaikkuna.

1. Jos haluat lisätä palvelun määritykset, sinun on luotava valmista määritystä kopio. Toiminto, valitse määrityksistä, jotka haluat kopioida nimi-luettelosta ja valitse sitten **Luo kopio**.

1. (Valinnainen) Anna palvelun toinen nimi, nimi-luettelosta uuden palvelun kokoonpanon ja valitse sitten **Nimeä uudelleen**. Kirjoita **nimi** -ruutuun nimi, jota haluat käyttää tämän palvelumäärityksen ja valitse sitten **OK**.

    Uusi, jonka nimi on ServiceConfiguration palvelun määritysten tiedosto. [Nimi] .cscfg lisätään ratkaisunhallinnassa Azure projektin.


### <a name="to-delete-a-service-configuration"></a>Jos haluat poistaa palvelun määritykset

1. Napsauta ratkaisunhallinnassa Avaa pikavalikko Azure projektin ja valitse sitten **Hallitse määrityksiä**.

    **Hallitse palvelun määritykset** -valintaikkuna.

1. Jos haluat poistaa palvelun määritykset, valitse määrityksistä, jotka haluat poistaa **nimi** -luettelosta ja valitse sitten **Poista**. Voit varmistaa, että haluat poistaa tämän määrityksen valintaikkuna.

1. Valitse **Poista**.

     Palvelun kokoonpanotiedosto poistetaan ratkaisunhallinnassa Azure projektin.


### <a name="to-rename-a-service-configuration"></a>Jos haluat nimetä uudelleen palvelun määritykset

1. Napsauta ratkaisunhallinnassa Avaa pikavalikko Azure projektin ja valitse sitten **Hallitse määrityksiä**.

    **Hallitse palvelun määritykset** -valintaikkuna.

1. Nimeä service-määritys uudelleen **nimi** -luettelosta uuden palvelun kokoonpanon ja valitse sitten **Nimeä uudelleen**. Kirjoita **nimi** -ruutuun nimi, jota haluat käyttää tämän palvelumäärityksen ja valitse sitten **OK**.

    Palvelun määritystiedoston nimeä muutetaan ratkaisunhallinnassa Azure Projectissa.

### <a name="to-change-a-service-configuration"></a>Jos haluat muuttaa palvelun määritykset

- Jos haluat muuttaa palvelun määritykset, Avaa pikavalikon haluat muuttaa Azure projektin rooli ja valitse sitten **Ominaisuudet**. Katso [Toimintaohje: roolien määrittäminen Azure-pilvipalvelussa Visual Studiossa](https://msdn.microsoft.com/library/azure/hh369931.aspx) lisätietoja.

## <a name="make-different-setting-combinations-by-using-profiles"></a>Tee eri asetusta kombinaatioiden profiileja käyttämällä

Profiilin avulla voit automaattisesti täyttää **Ohjatun julkaisutoiminnon** eri yhdistelmät asetusten kanssa eri tarkoitukseen. Esimerkiksi voi olla yksi profiilin virheenkorjaus ja muodostaa toiseen versioon. Siinä tapauksessa **Virheenkorjaus** profiilin on **IntelliTrace** käytössä ja valittuna **Virheenkorjaus** määritykset ja **Release** profiilin on **IntelliTrace** käytöstä ja valittuna **Release** -määritys. Eri profiilien avulla voi myös eri tallennustilan tilillä palvelun käyttöön.

Kun suoritat ohjatun toiminnon ensimmäistä kertaa, oletusprofiilin luodaan. Visual Studio tallentaa profiilin tiedosto sisältää .azurePubXml-tunniste, johon on lisätty projektiin Azure- **profiileista** -kansiossa. Jos määrität erilaisia vaihtoehtoja manuaalisesti, kun käynnistät ohjatun toiminnon myöhemmin, tiedosto päivitetään automaattisesti. Ennen kuin suoritat seuraavat toimet, sinun kannattaa on jo julkaistu pilvipalvelussa sijaitsevaan vähintään kerran.

### <a name="to-add-a-profile"></a>Jos haluat lisätä profiili

1. Avaa pikavalikko Azure projektin ja valitse sitten **Julkaise**.

1. Valitse **Tallenna profiili** -painike seuraavassa kuvassa **kohteen profiili** -luettelon vieressä. Tämä luo profiilin puolestasi.

    ![Luo uusi profiili](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/create-new-profile.png)

1. Kun profiili on luotu, valitse **<... hallinta >** **kohteen profiili** -luettelosta.

    **Profiilien hallinta** -valintaikkuna tulee näyttöön, kuin seuraavassa kuvassa.

    ![Hallinta-profiileista valintaikkunan](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-profiles.png)

1. Valitse **nimi** -luettelosta profiilinvalintakehotteen ja valitse sitten **Luo kopio**.

1. Valitse **Sulje** -painiketta.

    Uusi profiili näkyy profiilin kohdeluettelosta.

1. Valitse **kohteen profiili** -luettelosta juuri luomasi profiili. Ohjatun julkaisutoiminnon asetukset ovat tiedot sekä profiilista valinnat.

1. Valitse näytettävät ohjatun julkaisutoiminnon jokaiselle sivulle **Edellinen** - ja **Seuraava** -painikkeet ja muokkaamalla profiilia asetuksia. Lisätietoja on kohdassa [Azure sovelluksen ohjatun julkaisutoiminnon](http://go.microsoft.com/fwlink/p/?LinkID=623085) .

1. Kun olet mukauttanut asetuksia, valitse **Seuraava** palaa asetukset-sivulla. Profiilin tallennetaan, kun julkaiset palvelun käyttämällä seuraavia asetuksia, tai jos valitset **Tallenna** profiilit luettelon vieressä.

### <a name="to-rename-or-delete-a-profile"></a>Jos haluat nimetä uudelleen tai poistaa profiilin

1. Avaa pikavalikko Azure projektin ja valitse sitten **Julkaise**.

1. Valitse **Hallitse** **kohteen profiili** -luettelosta.

1. **Profiilien hallinta** -valintaikkunassa valitse profiili, jonka haluat poistaa, ja valitse sitten **Poista**.

1. Valitse vahvistus-valintaikkunassa valitse **OK**.

1. Valitse **Sulje**.

### <a name="to-change-a-profile"></a>Voit muuttaa profiilin

1. Avaa pikavalikko Azure projektin ja valitse sitten **Julkaise**.

1. Valitse profiili, jonka haluat muuttaa **kohteen profiili** -luettelosta.

1. Valitse näytettävät ohjatun julkaisutoiminnon jokaiselle sivulle **Edellinen** - ja **Seuraava** -painikkeet ja muuta haluamiasi asetuksia. Lisätietoja on kohdassa [Azure sovelluksen ohjatun julkaisutoiminnon](http://go.microsoft.com/fwlink/p/?LinkID=623085) .

1. Kun olet muuttanut asetuksia, valitse **Seuraava** palaa **asetukset** -sivulla.

1. (Valinnainen) Valitse **Julkaise** julkaista pilvipalvelussa, käyttämällä uusia asetuksia. Jos et halua julkaista pilvipalvelussa sijaitsevaan tällä hetkellä, voit sulkea ohjatun julkaisutoiminnon Visual Studio kysyy, haluatko tallentaa muutokset profiiliin.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja muiden osien Azure projektin Visual Studio määrittämisestä on artikkelissa [Azure-projektin määrittäminen](http://go.microsoft.com/fwlink/p/?LinkID=623075)
