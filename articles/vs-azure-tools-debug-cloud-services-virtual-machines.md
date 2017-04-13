<properties 
    pageTitle="Virheenkorjaus Azure pilvipalveluun tai virtual machine Visual Studiossa | Microsoft Azure"
    description="Virheenkorjaus pilvipalveluun tai Virtual Machine Visual Studiossa"
    services="visual-studio-online"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />
<tags 
    ms.service="visual-studio-online"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/15/2016"
    ms.author="tarcher" />

# <a name="debugging-an-azure-cloud-service-or-virtual-machine-in-visual-studio"></a>Virheenkorjaus Azure pilvipalveluun tai virtual machine Visual Studiossa

Visual Studio saat virheenkorjaus Azure pilvipalveluiden ja näennäiskoneiden eri asetuksia.



## <a name="debug-your-cloud-service-on-your-local-computer"></a>Korjaa oman pilvipalvelussa paikallisessa tietokoneessa

Voit säästää aikaa ja raha Azure käyttämällä Laske emulaattorin virheenkorjaus oman pilvipalvelussa paikallisessa tietokoneessa. Virheenkorjaus palvelu paikallisesti ennen niiden käyttöönottoa, voit parantaa luotettavuutta ja suorituskykyä maksamatta suorittaminen kerran. Kuitenkin joitakin virheitä saattaa ilmetä vain kun suoritat pilvipalveluun Azure itse. Voit korjata virheet Jos otat remote virheenkorjaus, kun palvelua ja liittää sitten virheenkorjaus roolin esiintymä.

Emulaattori varmistaminen Azure Laske-palvelun ja suorittaa paikallisen ympäristön, niin, että voit testata ja virheenkorjaus pilvipalvelussa sijaitsevaan ennen niiden käyttöönottoa. Emulaattori käsittelee roolin esiintymät elinkaari ja Simuloitu resurssit, kuten paikallisen tallennustilan käyttö. Kun virheenkorjaus tai palvelun käynnistäminen Visual Studiossa, käynnistyy automaattisesti taustan sovelluksena emulaattori ja ottaa sitten käyttöön palvelun emulaattori. Emulaattori avulla voit tarkastella palvelun, kun se suoritetaan paikallisessa ympäristössä. Voit suorittaa täyden version tai emulaattori express versio. (Alkaen Azure 2.3 emulaattori express versio on oletusarvo.) Katso [käyttämällä emulaattorin Express Suorita ja korjaamisessa pilvipalveluun paikallisesti](https://msdn.microsoft.com/library/dn339018.aspx).

### <a name="to-debug-your-cloud-service-on-your-local-computer"></a>Voit korjata oman pilvipalvelussa paikalliseen tietokoneeseen

1. Valitse valikkoriviltä **Virheenkorjaus**, **Käynnistä virheenkorjaus** , jotta voit suorittaa Azure cloud palvelun projektin. Vaihtoehtoisesti voit painaa F5-näppäintä. Näyttöön tulee sanoma, joka Laske emulaattori käynnistyy. Emulaattori käynnistyessä ilmaisinalueen kuvake vahvistaa sen.

    ![Azure emulaattorin ilmaisinalueessa](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC783828.png)

1. Näyttää käyttöliittymän Laske emulaattori avaamalla pikavalikosta Azure-kuvake ilmaisinalueella ja valitse sitten **Näytä Laske emulaattorin-Käyttöliittymä**.

    Käyttöliittymän vasemmassa ruudussa näkyy palvelut, jotka on otettu käyttöön ja Laske emulaattori roolin esiintymät, jossa on kunkin palvelun. Voit valita palvelun tai Näytä elinkaari, kirjaaminen ja vianmääritystiedot oikeanpuoleisen ruudun roolit. Jos Aktivoi yläreunuksen mukana ikkunan, saat oikeanpuoleisessa ruudussa täyttö.

1. Askel sovellus valitsemalla **Virheenkorjaus** -valikon ja määrittämällä keskeytyskohdat koodisi. Kun virheenkorjaus sovelluksen askel-ruutujen päivittyvät sovelluksen nykyisen tilan. Kun lopetat virheenkorjaus-sovelluksen asennus poistetaan. Jos olet määrittänyt käynnistys action-ominaisuuden arvoksi Käynnistä selain sovelluksen sisältää web-rooli, Visual Studio käynnistyy web-sovelluksen selaimessa. Jos muutat palvelun roolia esiintymien määrän, pilvipalvelussa sijaitsevaan Lopeta ja Käynnistä virheenkorjaus niin, että nämä uusia esiintymiä roolin voit korjata virheet.

    **Huomautus:** Kun lopetat käynnissä tai palvelusi virheenkorjaus, paikallinen Laske emulointikoneen ja tallennustilaa emulaattorin eivät ole pysäyttää. On lopettaa ne eksplisiittisesti ilmaisinalueelta.


## <a name="debug-a-cloud-service-in-azure"></a>Virheenkorjaus pilvipalvelussa Azure-tietokannassa

Korjaamisessa pilvipalvelussa remote tietokoneesta, sinun on otettava vastaavia toimintoja eksplisiittisesti kun otat käyttöön oman pilvipalvelussa, niin, että tarvitaan palveluita (esimerkiksi msvsmon.exe), jotka suoritetaan roolin esiintymät näennäiskoneiden on asennettu. Jos et käyttöön remote virheenkorjaus, kun palvelua julkaistu, sinun on julkaista palvelun kanssa remote virheenkorjaus käytössä.

Jos otat remote virheenkorjaus pilvipalveluun, se toimia eivät toimi oikein suorituskyvyn tai lisämaksuja. Ei kannata käyttää remote virheenkorjaus tuotannon-palveluun, koska asiakkaat-palvelua käyttävät saattaa heikentyä.

>[AZURE.NOTE] Kun julkaiset Visual Studio pilvipalveluun, voit ottaa **IntelliTrace** minkä tahansa palvelun roolien, .NET Framework 4 tai .NET Framework 4.5. Käyttämällä **IntelliTrace**tutkia roolin esiintymän on tapahtunut aiemman ja toistaa rivin kontekstissa, ajasta. Katso [Virheenkorjaus julkaistun pilvipalvelussa IntelliTrace ja Visual Studio](http://go.microsoft.com/fwlink/?LinkID=623016) ja [Käyttämällä IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx).

### <a name="to-enable-remote-debugging-for-a-cloud-service"></a>Jos haluat ottaa käyttöön remote virheenkorjaus pilvipalveluun

1. Avaa pikavalikko Azure projektin ja valitse sitten **Julkaise**.

1. Valitse **väliaikaisen** -ympäristön ja **Korjaa** -määritys.

    Tämä on vain yleisohjeet. Voit valita, Suorita testi-ympäristöissä tuotantoympäristössä. Voivat vaikuttaa vaikuttaa käyttäjiin Jos otat remote virheenkorjaus tuotantoympäristössä. Voit valita Release määritykset, mutta virheenkorjaus-kokoonpano tekee virheenkorjaus helpottuu.

    ![Valitse virheenkorjaus-määritys](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746717.gif)

1. Tavanomainen noudattamalla, mutta valitse **Lisäasetukset** -välilehden **Ottaminen käyttöön Remote virheenkorjaus kaikki roolit** -valintaruutu.

    ![Virheenkorjaus määritys](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746718.gif)

### <a name="to-attach-the-debugger-to-a-cloud-service-in-azure"></a>Jos haluat liittää virheenkorjaus pilvipalveluun Azure-tietokannassa

1. Laajenna pilvipalvelussa sijaitsevaan solmu palvelimen Resurssienhallinnassa.

1. Avaa pikavalikko roolin tai roolin esiintymän, johon haluat liittää, ja valitse sitten **Liitä virheenkorjaus**.

    Jos korjaat rooli, Visual Studio virheenkorjaus liittää roolin esiintymissä. Virheenkorjaus katkaisee keskeytyskohdasta ensimmäisen esiintymän rooli, joka suorittaa koodin riville ja täyttävät kaikki kyseisen keskeytyskohdasta ehdot. Jos korjaat erillisen virheenkorjaus liittää vain kyseisen esiintymän ja sivunvaihdot-keskeytyskohdasta vain silloin, kun tietyn tekstikohdan suorittaa koodin riville ja vastaa keskeytyskohdasta ehdot.

    ![Liitä virheenkorjaus](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746719.gif)

1. Kun virheenkorjaus liittää erillisen, korjata tavalliseen tapaan. Virheenkorjaus liittää automaattisesti roolin tarvittavat host-prosessi. Sen mukaan, mitä rooli on virheenkorjaus liittää w3wp.exe, WaWorkerHost.exe tai WaIISHost.exe. Voit varmistaa, johon on liitetty virheenkorjaus prosessi, laajenna palvelimen Explorer esiintymä-solmu. Saat lisätietoja Azure prosessit [Azure roolin arkkitehtuuri](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) .

    ![Valitse koodi tyyppi-valintaikkuna](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Voit selvittää, johon on liitetty virheenkorjaus prosessit-prosessien valintaikkunan lajitteluperuste-palkissa, valitsemalla virheenkorjaus-, Windows- ja prosessit-valikon avaaminen (Näppäimistön: Ctrl + Alt + Z) Irrota prosessi, Avaa pikavalikoissa ja valitse sitten **Irrottaa prosessi**. Tai Etsi solmun esiintymän palvelimen Resurssienhallinnassa, Etsi prosessi, Avaa pikavalikoissa ja valitse sitten **Irrottaa prosessi**.

    ![Virheenkorjaus prosessit](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC690787.gif)

>[AZURE.WARNING] Vältä pitkä sarkaimet keskeytyskohdat kun remote on virheenkorjaus. Azure käsittelee prosessi, jossa on pysäytetty kauemmin kuin vastaamisen muutama minuutti ja lopettaa liikenne lähettää kyseisen esiintymän. Jos lopetat liian kauan, msvsmon.exe liityntäkohtaan prosessi.

Irrota virheenkorjaus-prosessien esiintymän tai rooli, Avaa pikavalikko roolin tai esiintymään, jota olet virheenkorjaus ja valitse sitten **Irrottaa virheenkorjaus**.

## <a name="limitations-of-remote-debugging-in-azure"></a>Rajoitukset remote virheenkorjaus Azure-tietokannassa

FROM Azure SDK 2.3 remote virheenkorjaus on seuraavat rajoitukset.

- Kanssa remote virheenkorjaus käytössä, ei voi julkaista pilvipalvelussa, missä tahansa roolilla on enintään 25 esiintymät.

- Virheenkorjaus käyttää portit 30400 30424, 31400 31424 ja 32400 32424. Jos yrität käyttää mitä tahansa porttien, et voi julkaista palvelun ja jompikumpi seuraavista virhesanomista näkyvät tehtävän lokin Azure: 

    - Virhe tarkistettaessa vastaan .csdef tiedoston .cscfg-tiedosto. 
    Varattu portti alue "alue" päätepisteen Microsoft.WindowsAzure.Plugins.RemoteDebugger.Connector roolin 'roolin' päällekkäinen jo porttiin tai alue.
    - Varaus epäonnistui. Yritä uudelleen myöhemmin, Vähennä AM koon tai rooli kerrat tai yritä käyttöönotossa toisella alueella.


## <a name="debugging-azure-virtual-machines"></a>Virheenkorjaus Azure-virtuaalikoneissa

Voit korjata käynnissä olevat Azuren näennäiskoneiden palvelimen Resurssienhallinnassa Visual Studiossa ohjelmat. Kun otat remote virheenkorjaus Azure virtuaalikoneen, Azure asentaa remote muistin tunniste virtuaalikoneen. Sitten voit liittää virtuaalikoneen prosesseja ja virheenkorjaus tavalliseen tapaan.

>[AZURE.NOTE] Luotu Azure resurssien hallinnan Selaa näennäiskoneiden voit korjattava etäyhteyden Cloud Resurssienhallinnassa Visual Studio 2015. Lisätietoja on artikkelissa [Azure resurssien hallinta Cloud Resurssienhallinnassa](http://go.microsoft.com/fwlink/?LinkId=623031).

### <a name="to-debug-an-azure-virtual-machine"></a>Jos haluat korjata Azure virtuaalikoneen

1. Palvelimen Resurssienhallinnassa Laajenna näennäiskoneiden-solmu ja valitse virtuaalikoneen, jonka haluat korjata solmu.

1. Avaa pikavalikkoa ja valitse **Ota käyttöön virheenkorjaus**. Kun sinulta kysytään, jos olet varma, jos haluat ottaa käyttöön virheenkorjaus-virtuaalikoneen, valitse **Kyllä**.

    Azure asentaa remote muistin tunniste virtuaalikoneen käyttöön virheenkorjaus.

    ![Virtuaalikoneen käyttöön muistin komento](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Azure tehtävän loki](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)

1. Kun remote muistin tunniste on päättynyt, Avaa virtual machine-pikavalikkoa ja valitse **Liitä virheenkorjaus...**

    Azure saa prosessit luettelo virtuaalikoneen ja ne näkyvät prosessi-valintaikkunassa Liitä.

    ![Liitä virheenkorjaus-komento](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)

1. Valitse **Liitä prosessi** -valintaikkunassa voit rajoittaa tulokset-luettelo näyttää koodia haluat korjata tyypit **Valitse** . Voit korjata 32 - tai 64-bittinen hallitun koodin tai alkuperäisen koodi.

    ![Valitse koodi tyyppi-valintaikkuna](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Valitse prosessit virheenkorjaus-virtuaalikoneen ja valitse sitten **Liitä**. Voit esimerkiksi valita w3wp.exe-prosessi, jos haluat määrittää virheenkorjaus-virtuaalikoneen verkkosovellukseen. Lisätietoja on kohdassa [korjaaminen yksi tai useampia prosesseja Visual Studiossa](https://msdn.microsoft.com/library/jj919165.aspx) ja [Azure roolin arkkitehtuuri](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) .

## <a name="create-a-web-project-and-a-virtual-machine-for-debugging"></a>Web-projekti ja käyttöympäristön virheenkorjaus luominen

Ennen kuin julkaiset Azure projektin hyödyllisiä se testaa sisältämät ympäristössä, joka tukee virheenkorjaus ja tilanteiden ja Täällä voit asentaa testaaminen seurantaan ja valvontaan ohjelmat. Yksi tapa on virheenkorjaus etäyhteyden sovelluksen virtual koneeseen.

Visual Studio ASP.NET-projektien tarjoa kätevää virtual-tietokoneeseen, voit käyttää sovelluksen testikäyttöön luomiseen. Virtuaalikoneen on yleisesti tarvitaan päätepisteet, kuten PowerShell, Etätyöpöytä ja WebDeploy.

### <a name="to-create-a-web-project-and-a-virtual-machine-for-debugging"></a>Web-projekti ja käyttöympäristön virheenkorjaus luomiseen

1. Visual Studiossa Luo uusi ASP.NET Web-sovelluksen.

1. Valitse uusi ASP.NET-projekti-valintaikkunassa Azure-osassa **virtuaalikoneen** avattavassa luettelossa. Jätä **Luo remote resurssit** -valintaruutu on valittuna. Valitse **OK** , jos haluat jatkaa.

    **Luo virtual tietokoneen Azure** -valintaikkuna.


    ![Luo ASP.NET web projekti-valintaikkuna](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746723.png)

    **Huomautus:** Sinua pyydetään kirjautumaan Azure-tili, jos et ole vielä kirjautunut.

1. Valitse virtuaalikoneen eri asetukset ja valitse sitten **OK**. Lisätietoja on kohdassa [näennäiskoneiden]( http://go.microsoft.com/fwlink/?LinkId=623033) .

    DNS-nimen kirjoittamasi nimi on virtuaalikoneen nimi. 

    ![Luo virtuaalikoneen Azure-valintaikkunassa](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746724.png)

    Azure Luo virtuaalikoneen ja sitten määräysten ja määrittää päätepisteet, kuten Etätyöpöytä ja Web käyttöönotto



1. Kun virtuaalikoneen on täysin määritetty, valitse virtual machine solmu palvelimen Resurssienhallinnassa.

1. Avaa pikavalikkoa ja valitse **Ota käyttöön virheenkorjaus**. Kun sinulta kysytään, jos olet varma, jos haluat ottaa käyttöön virheenkorjaus-virtuaalikoneen, valitse **Kyllä**. 

    Azure asentaa remote muistin tunniste virtuaalikoneen käyttöön virheenkorjaus.

    ![Virtuaalikoneen käyttöön muistin komento](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Azure tehtävän loki](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)

1. Julkaista projektisi, kuvatulla tavalla [Toimintaohje: käyttöönotto Web projektin käyttämällä yhdellä napsautuksella julkaiseminen Visual Studiossa](https://msdn.microsoft.com/library/dd465337.aspx). Koska haluat korjata virtual tietokoneessa, valitse **asetukset** -sivun **Sivuston julkaiseminen** ohjatun toiminnon Valitse määritykset **Virheenkorjaus** . Näin varmistat, että koodi merkit ovat käytettävissä korjattaessa.

    ![Julkaisuasetukset](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718349.png)

1. Valitse **Tiedoston Julkaisuasetukset** **Poista kohde muita tiedostoja** , jos projektin Oletko jo ottanut käyttöön aiemmin.

1. Kun projektin julkaisee virtual machine pikavalikko palvelimen Resurssienhallinnassa valitsemalla **Liitä virheenkorjaus...**

    Azure saa prosessit luettelo virtuaalikoneen ja ne näkyvät prosessi-valintaikkunassa Liitä.

    ![Liitä virheenkorjaus-komento](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)

1. Valitse **Liitä prosessi** -valintaikkunassa voit rajoittaa tulokset-luettelo näyttää koodia haluat korjata tyypit **Valitse** . Voit korjata 32 - tai 64-bittinen hallitun koodin tai alkuperäisen koodi.

    ![Valitse koodi tyyppi-valintaikkuna](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Valitse prosessit virheenkorjaus-virtuaalikoneen ja valitse sitten **Liitä**. Voit esimerkiksi valita w3wp.exe-prosessi, jos haluat määrittää virheenkorjaus-virtuaalikoneen verkkosovellukseen. Lisätietoja on kohdassa [korjaaminen yksi tai useampia prosesseja Visual Studiossa](https://msdn.microsoft.com/library/jj919165.aspx) .

## <a name="next-steps"></a>Seuraavat vaiheet

- **Intellitrace** avulla voit kerätä puhelut ja tapahtumien loki release-palvelimesta. Katso [Virheenkorjaus julkaistun pilvipalvelussa IntelliTrace ja Visual Studio](http://go.microsoft.com/fwlink/?LinkID=623016).
- Kirjaudu yksityiskohtaiset tiedot sisällä roolit-koodin, onko roolit ovat käynnissä kehitysympäristö tai Azure **Azure diagnostiikan** avulla. Katso [kerääminen kirjaaminen tietojen Azure Diagnostiikan avulla](http://go.microsoft.com/fwlink/p/?LinkId=400450).
