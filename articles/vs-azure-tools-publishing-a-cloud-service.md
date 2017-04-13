<properties
   pageTitle="Julkaiseminen Azure-työkaluilla pilvipalveluun | Microsoft Azure"
   description="Tietoja siitä, miten voit julkaista Azure cloud palvelun projektien Visual Studion avulla."
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

# <a name="publishing-a-cloud-service-using-the-azure-tools"></a>Azure-työkaluilla pilvipalveluun julkaiseminen

Voit julkaista Azure sovelluksen suoraan Visual Studio käyttämällä Microsoft Visual Studio Azure-työkaluja. Visual Studio tukee integroitu julkaiseminen väliaikaisen tai pilvipalveluun tuotantoympäristössä.

Ennen kuin voit julkaista Azure sovelluksen, sinulla on oltava Azure tilaus. On määritettävä cloud-palveluun ja tallennustilaa tilin käytettävän sovelluksen. Voit määrittää nämä [Azure perinteinen portaalissa](http://go.microsoft.com/fwlink/?LinkID=213885)osoitteessa.

>[AZURE.IMPORTANT] Kun julkaiset, voit valita käyttöönotto-ympäristön cloud-palveluun. Valittava myös tallennustilan-tili, jota käytetään käyttöönottoa varten sovelluspaketin tallentamiseen. Käyttöönoton jälkeen sovelluspaketin poistetaan tallennustilan-tililtä.

Kun kehität ja testaus Azure sovelluksen, voit käyttää julkaisemaan muutokset vaiheittainen web-roolien Web käyttöönotto. Kun julkaiset sovelluksen käyttöönotto-ympäristöön, Web-käyttöön voit Ota muutokset käyttöön suoraan virtuaalikoneen, jossa on käytössä web rooli. Sinun ei tarvitse pakkaaminen ja julkaista koko Azure sovelluksen aina, kun haluat päivittää web-tehtäväsi testata muutokset. Tämän menetelmän käytössä voi olla web roolin muutokset käytettävissä testikäyttöön odottamatta on julkaistu käyttöönotto-ympäristöön sovelluksen pilveen.

Seuraavien ohjeiden avulla voit julkaista Azure sovelluksesi ja päivittää web-roolin käyttämällä Web käyttöön:

- Julkaista tai pakata Azure sovelluksen Visual Studio

- Päivitä verkko-roolin osana kehittäminen ja testauksen kehä

## <a name="publish-or-package-an-azure-application-from-visual-studio"></a>Julkaista tai pakata Azure sovelluksen Visual Studio

Kun julkaiset Azure sovelluksesi, tee jokin seuraavista toimista:

- Palvelun paketin luominen: Voit julkaista sovelluksen käyttöönotto-ympäristöön [Azure perinteinen Portal](http://go.microsoft.com/fwlink/?LinkID=213885)-paketin ja palvelun kokoonpanotiedosto.

- Julkaista projektisi Azure Visual Studio: Voit julkaista suoraan Azure sovellusta, voit käyttää ohjatun julkaisutoiminnon. Lisätietoja on artikkelissa [Azure sovelluksen ohjatun julkaisutoiminnon](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-create-a-service-package-from-visual-studio"></a>Visual Studio service-paketin luominen

1. Kun olet valmis julkaisemaan sovelluksen, Avaa ratkaisunhallinnassa, Avaa pikavalikko Azure projektin, joka sisältää oman roolit ja valitse Julkaise.

1. Jos haluat luoda vain palvelupakettiin, toimi seuraavasti:  

  1. Valitse pikavalikosta Azure projektin **paketti**.

  1. **Paketin Azure sovellus** -valintaikkunassa valitse palvelun asetuksiin, johon haluat luoda paketin ja valitse sitten Muodosta-määritys.

  1. (valinnainen) Ottaa käyttöön etätyöpöytä pilvipalvelussa sijaitsevaan julkaisemisen jälkeen, valitse **Ota käyttöön kaikki roolit Etätyöpöytä** -valintaruutu ja valitse sitten etätyöpöytä **asetukset** . Jos haluat korjata pilvipalvelussa sijaitsevaan julkaisemisen jälkeen, ota käyttöön valitsemalla **Ota käyttöön Remote virheenkorjaus rooleista**remote virheenkorjaus.

      Lisätietoja on artikkelissa [Käyttämällä etätyöpöydän Azure roolien](vs-azure-tools-remote-desktop-roles.md).

  1. Voit luoda paketin, valitse **paketti** -linkki.

      Resurssienhallinta näkyy juuri luomasi paketin tiedostosijainti. Voit kopioida tähän sijaintiin niin, että voit käyttää sitä [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885).

  1. Tämän paketin julkaiseminen käyttöönotto-ympäristössä, sinun on käytettävä tämän sijainnin paketin sijainniksi, kun luot pilvipalveluun ja tämän paketin käyttöönotto [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885)-ympäristössä.

1. (Valinnainen) Peruuta käyttöönottoprosessin pikavalikon rivin kohteen tehtävän lokiin, valitse **Peruuta ja poista**. Tämä lopettaa käyttöönottoprosessin ja poistaa Azure käyttöönotto-ympäristössä.

    >[AZURE.NOTE] Jos haluat poistaa käyttöönotto-ympäristössä, sen jälkeen, kun se on otettu käyttöön, sinun on käytettävä [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885).

1. (Valinnainen) Kun rooli esiintymät on jo alkanut, Visual Studio näkyy automaattisesti käyttöönotto-ympäristön **Cloud Services** -solmu palvelimen Resurssienhallinnassa. Tässä näet yksittäisen roolin esiintymien tila. Katso [hallinta Azure resurssien Cloud Resurssienhallinnassa](vs-azure-tools-resources-managing-with-cloud-explorer.md). Seuraavassa kuvassa on roolin esiintymät silloin, kun ne ovat edelleen alustaminen onnistuu tila:

    ![VST_DeployComputeNode](./media/vs-azure-tools-publishing-a-cloud-service/IC744134.png)

## <a name="update-a-web-role-as-part-of-the-development-and-testing-cycle"></a>Päivitä verkko-roolin osana kehittäminen ja testauksen kehä

Jos sinua sovelluksen Taustajärjestelmä infrastruktuurin säilyy, mutta verkko-roolien on enemmän päivittää usein, voit tehdä Web käyttöönotto päivittää vain web-roolin projektin. Tämä on kätevä, jos et halua muodostaa ja ota Taustajärjestelmä työntekijä roolit uudelleen tai jos sinulla on useita web-rooleja ja haluat päivittää vain yksi web-roolit.

### <a name="requirements"></a>Vaatimukset

Seuraavassa on Web käyttöönotto avulla voit päivittää web-roolin vaatimukset:

- **Kehittämisen ja testaus ilmoitusta:** Muutokset tehdään suoraan virtuaalikoneen, jossa web rooli on käynnissä. Jos virtual tässä tietokoneessa on oltava kuluessa, muutokset menetetään, koska alkuperäisen paketin, jossa on julkaistu käytetään luovan virtuaalikoneen oikeuksia. Saat uusimmat muutokset web-roolin sovelluksen täytyy julkaista uudelleen.

- **Web-roolit ovat päivitettäviä:** Työntekijän roolia ei voi päivittää. Et voi päivittää RoleEntryPoint-web-role.cs.

- **Tukee vain yhden esiintymän web-roolin:** Ei voi olla mikä tahansa verkko-roolin useita kertoja käyttöönotto-ympäristössä. Kuitenkin useita web rooleja kunkin vain yhtä esiintymää tuetaan.

- **Sinun on otettava työpöydän etäyhteyksien:** Tämä tarvitaan, jotta Web käyttöönotto käyttää käyttäjä- ja salasana muodostaa virtuaalikoneen ottamaan muutokset palvelimeen, joka toimii Internet Information Services (IIS). Lisäksi voit joutua muodostamaan yhteys virtuaalikoneen luotettu varmenteen lisääminen IIS virtual tässä tietokoneessa. (Näin varmistat, että etäyhteyden IIS, jota käytetään Web käyttöönotto on suojattu.)

Seuraavassa oletetaan, että käytössäsi on **Julkaista Azure-sovelluksen** ohjatun toiminnon.

### <a name="to-enable-web-deploy-when-you-publish-your-application"></a>Voit ottaa käyttöön julkaiset sovelluksen Web käyttöönotto

1. Voit ottaa käyttöön **Ota käyttöön Web käyttöön** kaikille web roolit-valintaruutua, sinun on ensin määritettävä työpöydän etäyhteyksien. Valitse **Ota käyttöön etätyöpöytä** rooleista ja anna tunnistetiedot, joita käytetään yhteyden etäyhteyden tulevassa **Remote Desktop-määritys** -valintaikkunassa. Lisätietoja on kohdassa [Käyttämällä etätyöpöydän Azure roolien](vs-azure-tools-remote-desktop-roles.md) .

1. Käyttöön Web käyttöön kaikkien web-sovelluksen roolien valitsemalla **Ota käyttöön Web käyttöön web rooleista**.

    Näkyy keltainen varoitus kolmio. Web-käyttöönotto käyttää luotettu, itse allekirjoitetun varmenteen oletusarvoisesti, joka ei ole suositeltavaa ladataan luottamuksellisia tietoja. Jos haluat suojata tämä prosessi luottamuksellisia tietoja, voit lisätä SSL-varmenne, jota käytetään Web käyttöönotto yhteydet. Tämän todistuksen on oltava luotettu varmenteen. Lisätietoja hallinnasta on kohdassa **Voit tehdä Web käyttöönotto suojatun** ohjeaiheen.

1. Valitse **Seuraava** Näytä **Yhteenveto** -ruutu ja valitse sitten **Julkaise** käyttöönotto pilvipalvelussa.

    Cloud-palvelu on julkaistu. Virtuaalikoneen, joka on luotu on etäyhteyksien käytössä IIS niin, että Web käyttöönotto voidaan päivittää web-roolien ilman julkaisemalla ne uudelleen.

    >[AZURE.NOTE] Jos sinulla on useita web-roolin määritetty esiintymän, näyttöön tulee varoitussanoma, jossa ilmoitetaan, että kunkin web-rooli on rajoitettu vain-paketti, joka on luotu, voit julkaista sovelluksesi yhdessä paikassa. Jatka valitsemalla **OK** . Ilmoitetulla tavalla koskevat vaatimukset-kohdassa voit määrittää useita web-rooli, mutta vain yhden esiintymän rooleille.

### <a name="to-update-your-web-role-by-using-web-deploy"></a>Päivitä tehtäväsi Web käyttämällä Web käyttöönotto

1. Web ottaa käyttöön muutosten koodin projektin minkään web-roolien Visual Studiossa, jonka haluat julkaista, ja napsauta hiiren kakkospainikkeella tämä ratkaisu projektin solmu ja **Julkaise**. **Julkaise** -valintaikkuna.

1. (Valinnainen) Jos lisäsit luotettujen SSL-varmenteen IIS etäyhteyksien käytettävät, voit poistaa **Salli luotettu varmenteen** -valintaruutu. Katso tietoja varmenteen tehdä Web käyttöönotto suojatun lisäämisestä on tämän artikkelin **Voit tehdä Web käyttöönotto suojatun** osan.

1. Julkaise-järjestelmä on käyttäjänimi ja salasana, jonka olet määrittänyt, että Etätyöpöytäyhteys kun paketti julkaistu Web ottaa käyttöön.

  1. Kirjoita **käyttäjänimi**-kenttään käyttäjänimi.

  1. Kirjoita **salasana**salasana.

  1. (Valinnainen) Jos haluat tallentaa salasanan profiilia, valitse **Tallenna salasana**.

1. Julkaiseminen web-roolin muutokset, valitse **Julkaise**.

    Tilarivillä näkyy **Julkaise alkuun**. Kun julkaisu on valmis, **Julkaise onnistui** tulee näkyviin. Muutokset on nyt otettu virtuaalikoneen web-roolin. Voit nyt aloittaa Azure sovelluksesi voit esikatsella muutoksiasi Azure-ympäristössä.

### <a name="to-make-web-deploy-secure"></a>Jotta suojatun Web käyttöönotto

1. Web-käyttöönotto käyttää luotettu, itse allekirjoitetun varmenteen oletusarvoisesti, joka ei ole suositeltavaa ladataan luottamuksellisia tietoja. Jos haluat suojata tämä prosessi luottamuksellisia tietoja, voit lisätä SSL-varmenne, jota käytetään Web käyttöönotto yhteydet. Tämän todistuksen on oltava luotettu varmenteen, jonka saat varmenteiden myöntäjältä (CA).

    Jotta Web käyttöönotto suojatun kunkin virtuaalikoneen kunkin web-roolien, lataa luotettu varmenteen, jota haluat käyttää sivuston [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885)käyttöönotto. Näin varmistat, että sertifikaatti lisätään virtuaalikoneen, joka on luotu web-roolin, kun julkaiset sovelluksen.

1. Voit lisätä luotetun SSL-varmenteen IIS etäyhteyksien käytettävät, toimi seuraavasti:

  1. Muodostaa yhteyden, jossa on käytössä web-roolin virtuaalikoneen Valitse web-roolin esiintymä **Cloud Explorer** tai **Palvelimen Resurssienhallinnassa**ja valitse sitten **Yhdistä käyttäen Etätyöpöytä** -komento. Yksityiskohtaisia ohjeita siitä, miten voit muodostaa yhteyden virtuaalikoneen on artikkelissa [Käyttämällä etätyöpöydän Azure roolien](vs-azure-tools-remote-desktop-roles.md).

      Selain pyytää sinua lataamaan. RDP-tiedostoon.

  1. Voit lisätä SSL-varmenteen avaamalla management-palvelu IIS hallinta. IIS hallinta käyttöön SSL avaamalla **toimintoruudun** **sidontojen** -linkkiä. **Lisää sivuston sidonta** -valintaikkunassa näkyy. Valitse **Lisää**ja valitse sitten HTTPS **tyyppi** avattavassa luettelossa. Valitse **SSL-varmenne** -luettelossa SSL-varmenne, oli allekirjoitettu varmenteen myöntäjä ja ladata [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885). Lisätietoja on artikkelissa [Management-palvelu yhteyden asetusten määrittäminen](http://go.microsoft.com/fwlink/?LinkId=215824).

      >[AZURE.NOTE] Jos lisäät luotettujen SSL-varmenne, keltainen varoitus kolmio ei enää näy **Ohjatun julkaisutoiminnon**.

## <a name="include-files-in-the-service-package"></a>Sisällytä tiedostot palvelupakettiin

Voit joutua muuttamaan tiettyjen tiedostojen sisällyttäminen service-paketti, niin, että ne ovat käytettävissä, jotka on luotu roolin virtuaalikoneen. Haluat esimerkiksi lisätä .exe tai .msi-tiedostoa, jota käytetään käynnistyskomentosarjan service-pakettiin. Tai voit joutua lisäämään kokoonpano, joka edellyttää web rooli tai työntekijän rooli projektin. Jos haluat lisätä ne on lisättävä ratkaisu Azure-sovelluksen tiedostot.

### <a name="to-include-files-in-the-service-package"></a>Sisällytettävien tiedostot palvelupakettiin

1. Jos haluat lisätä kokoonpanon service-paketti, käytä seuraavasti:

  1. Avaa projekti, jota ei ole viitattua kokoonpanoa projektin solmu **Ratkaisunhallinnassa** .

  1. Voit lisätä projektin kokoonpano, Avaa pikavalikko **viittaukset** -kansion ja valitse sitten **Lisää viittaus**. Lisää viittaus-valintaikkuna tulee näyttöön.

  1. Valitse muutettava viittaus, jonka haluat lisätä, ja valitse sitten **OK** -painiketta.

      Viittaus lisätään **viittaukset** -kansio-kohdan luettelosta.

  1. Avaa pikavalikko, jonka lisäsit kokoonpanon ja valitse **Ominaisuudet**. **Ominaisuudet** -ikkuna.

      Sisällytettävät palvelupakettiin **paikallisen kopion luettelon** tämän kokoonpanon Valitse **Tosi**.

1. Avaa projekti, jota ei ole viitattua kokoonpanoa projektin solmu **Ratkaisunhallinnassa** .

1. Voit lisätä projektin kokoonpano, Avaa pikavalikko **Viitteet** -kansion ja valitse sitten **Lisää viittaus**. **Lisää viittaus** -valintaikkuna tulee näyttöön.

1. Valitse muutettava viittaus, jonka haluat lisätä, ja valitse sitten **OK** -painiketta.

    Viittaus lisätään **viittaukset** -kansio-kohdan luettelosta.

1. Avaa pikavalikko, jonka lisäsit kokoonpanon ja valitse **Ominaisuudet**. Ominaisuudet-ikkuna.

1. Tämän kokoonpanon sisällytettävät palvelupakettiin **Paikallinen kopio** -luettelossa, valitse **Tosi**.

1. Sisällytettävien tiedostot, jotka on lisätty projektiin web-roolin palvelupakettiin Avaa pikavalikko tiedosto ja valitse sitten **Ominaisuudet**. Valitse **Ominaisuudet** -ikkunassa **sisällön** **Luominen toiminnon** luettelosta-ruutuun.

1. Sisällytettävien tiedostot, jotka on lisätty projektiin työntekijän rooli palvelupakettiin Avaa pikavalikko tiedosto ja valitse sitten **Ominaisuudet**. Valitse **Ominaisuudet** -ikkunassa **kopioiminen Jos uudempaan** **Kopioi tulosteen hakemisto** -luetteloruudussa.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja julkaiseminen ja Azure Visual Studio on artikkelissa [Azure sovelluksen ohjatun julkaisutoiminnon](vs-azure-tools-publish-azure-application-wizard.md).
