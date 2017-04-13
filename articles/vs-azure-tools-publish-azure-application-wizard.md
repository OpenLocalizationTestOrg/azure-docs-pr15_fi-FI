<properties 
   pageTitle="Azure sovelluksen Ohjattu julkaisutoiminto | Microsoft Azure"
   description="Azure sovelluksen Ohjattu julkaisutoiminto"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="publish-azure-application-wizard"></a>Azure sovelluksen Ohjattu julkaisutoiminto

## <a name="overview"></a>Yleiskatsaus

Kun kehität Visual Studiossa web-sovelluksen, voit julkaista sovelluksesta helpommin Azure pilvipalvelussa **Julkaista Azure-sovelluksen** ohjatun toiminnon avulla. Ensimmäinen osa kerrotaan vaiheet, sinun on suoritettava, ennen kuin käyttämällä ohjattua jäljellä kerrotaan ominaisuudet ohjatun toiminnon.

>[AZURE.NOTE] Tässä ohjeaiheessa on tietoja käyttöönotossa pilvipalvelut, ei ole sivustoja. Tietoja sivustojen käyttöönotossa on artikkelissa [Azure-Web-sivuston ottamisesta](https://social.msdn.microsoft.com/Search/windowsazure?query=How%20to%20Deploy%20an%20Azure%20Web%20Site&Refinement=138&ac=4#refinementChanges=117&pageNumber=1&showMore=false).

## <a name="prerequisites"></a>Edellytykset

Ennen kuin voit julkaista web-sovelluksen Azure, on oltava Microsoft-tili ja Azure tilaus ja sinun on liitettävä web-sovelluksen Azure pilvipalvelussa. Jos olet jo suorittanut nämä tehtävät, voit siirtyä seuraavaan osaan.

1. Hanki Microsoft-tili ja Azure tilauksen. Voit kokeilla vapaa kuukausi maksuttoman Azure tilauksen [tähän](https://azure.microsoft.com/pricing/free-trial/)

1. Luo pilvipalveluun ja tallennustilaa tilin Azure. Voit tehdä tämän Visual Studiossa tai [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885)Server Explorerista.

1. Ota käyttöön Azure verkkosovelluksen. Web-sovelluksen Azure Visual Studio julkaiseminen käyttöön sinun on liitettävä Azure cloud service-projektin Visual Studiossa. Luomiseen liittyvät cloud palvelun projektin avaaminen project web-sovelluksen pikavalikko ja valitse sitten Muunna, **Muunna Azure Cloud palvelun projektin**.

1. Kun cloud palvelun projekti on lisätty ratkaisuun, Avaa samasta pikavalikosta uudelleen ja valitse sitten **Julkaise**. Lisätietoja Azure-sovellusten käyttämisestä on artikkelissa [Toimintaohje: Siirrä ja julkaista Web-sovelluksen Azure pilvipalveluun Visual Studio](https://msdn.microsoft.com/library/azure/hh420322.aspx).

>[AZURE.NOTE] Muista Visual Studio aloittaminen järjestelmänvalvojan tunnistetiedot (Suorita järjestelmänvalvojana).

1. Kun olet valmis julkaisemaan sovelluksen, Avaa pikavalikko Azure cloud palvelun projektin ja valitse sitten **Julkaise**. Seuraavissa vaiheissa kuvataan julkaista Azure-sovelluksen ohjatun toiminnon.

## <a name="choosing-your-subscription"></a>Tilauksen valitseminen

### <a name="to-choose-a-subscription"></a>Valitse tilaus

1. Ennen kuin käytät ohjattua ensimmäistä kertaa, sinun on kirjauduttava. Valitse **Kirjaudu sisään** -linkki. Kirjaudu sisään pyydettäessä Azure portaaliin ja Azure käyttäjänimeä ja salasanaa. 

    ![Tämä on yksi julkaisun näytöt](./media/vs-azure-tools-publish-azure-application-wizard/IC799159.png)

    Tilaukset-luettelossa näkyvät tilisi tilaukset. Näet ehkä myös tilauksen tiedostoista, jotka olet tuonut aiemmin tilaukset.

1. Valitse **Valitse tilaus** -luettelosta tilauksen käyttämään tätä käyttöönottoa varten.

   Jos valitset **<... hallinta >**, **Tilausten hallinta** -valintaikkuna ja voit valita tilauksen ja käyttäjän tiliä, jota haluat käyttää. **Asiakkaat** -välilehdessä näkyvät kaikki tilit ja **tilaukset** -välilehdessä näkyvät kaikki tilit liittyvät tilaukset. Voit myös valita alue, josta Azure resursseissa sekä luoda tai tuoda varmenteet tilauksen Azure-portaalista. Jos olet tuonut tilauksia tilauksen tiedostosta, valitse **Varmenteet** -välilehti tulee näkyviin liittyvät varmenteet. Kun olet valmis, valitse **Sulje** -painiketta.

    ![Tilausten hallinta](./media/vs-azure-tools-publish-azure-application-wizard/IC799160.png)

    >[AZURE.NOTE] Tilauksen tiedoston voi olla useita tilauksia.

1. Valitse **Seuraava ja jatka** . 

    Jos tilaukseesi ole mitään pilvipalveluihin, tarvitset pilvipalveluun luominen Azure isännöimiseen projektin. **Luo pilvipalvelussa ja tallennustilaa tili** -valintaikkuna tulee näkyviin.

    Määritä uusi nimi pilvipalvelussa. Nimen on oltava yksilöllinen Azure-tietokannassa. Määritä alue tai affiniteetti ryhmän tietokeskuksen, joka on lähellä etkä Useimmat asiakkaat. Tätä nimeä käytetään myös uusi tallennustilan-tili, jonka Azure Luo cloud-palveluun.

1. Muokkaa asetuksia haluat tämän käyttöönottoa varten ja julkaista sen valitsemalla **Julkaise** -painikkeen (seuraavassa osassa on lisätietoja asetuksia). Tarkista ennen julkaisemista, valitsemalla **Seuraava** .

    >[AZURE.NOTE] Jos valitsit Julkaise tässä vaiheessa, voit valvoa tämän käyttöönotto Visual Studiossa tila.

Voit muokata asetukset Yleiset- ja Lisäasetukset käyttöönottoa **Julkaista Azure-sovelluksen** ohjatun toiminnon avulla. Voit valita esimerkiksi asetus käyttöön testiympäristössä sovellusta, ennen kuin vapautat sen. Seuraavassa kuvassa näkyy Azure käyttöönottoa varten **Yleiset asetukset** -välilehti.

![Yleiset asetukset](./media/vs-azure-tools-publish-azure-application-wizard/IC749013.png)

## <a name="configuring-your-publish-settings"></a>Määrittäminen oman Julkaisuasetukset

### <a name="to-configure-the-publish-settings"></a>Julkaise-asetusten määrittäminen

1. **Pilvipalvelussa** -luettelosta suorittaa seuraavat siirtämistä seuraavasti:

   1. Valitse avattavasta luetteloruudusta aiemmin pilvipalvelussa. Palvelun tiedot center-sijainti tulee näkyviin. Olisi merkitsemään tallennuspaikan muistiin ja varmista, että tilin tallennuspaikka on sama tietokeskuksen.

    1. Valitse **Luo uusi** luominen pilvipalveluun Azure isännöivä. **Luo pilvipalvelussa** -valintaikkunassa Anna palvelun nimi ja määritä alue tai Määritä sijainti, johon haluat isännöidä tämän pilvipalvelussa tietokeskuksen affiniteetti ryhmän. Nimen on oltava yksilöllinen Azure-tietokannassa.

1. Valitse **tuotannon** tai **väliaikaisesta** **ympäristön** -luettelossa. Jos haluat ottaa käyttöön testiympäristössä sovellusta, valitse väliaikaisen ympäristössä. Voit siirtää sovelluksen tuotantoympäristössä myöhemmin.

1. Valitse **Muodosta määritys** -luettelossa **Virheenkorjaus** tai **versiossa**.

1. Valitse **palvelun configuration** -luettelosta **Cloud** tai **Paikallinen**.

    Valitse **Ota käyttöön kaikki roolit Etätyöpöytä** -valintaruutu, jos haluat, että voit muodostaa etäyhteyden palvelu. Tämä vaihtoehto käytetään ensisijaisesti vianmääritystä varten. Kun valitset tämän valintaruudun, näkyviin tulee **Remote Desktop-määritys** -valintaikkuna. Valitse Asetukset-linkki, voit muuttaa määrityksiä.

    Valitse WWW-palvelun käyttöönoton käyttöön **Ottaminen käyttöön Web Ota käyttöön kaikki web roolit** -valintaruutu. Sinun on otettava etätyöpöydän kautta voit käyttää tätä toimintoa. Lisätietoja on artikkelissa [[Azure-työkaluilla pilvipalveluun julkaiseminen](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx). Saat lisätietoja Web käyttöönotto [[julkaiseminen pilvipalveluun Azure-työkalujen avulla](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx).

1. Valitse **Lisäasetukset** -välilehti. **Käyttöönoton otsikko** -kenttään Hyväksy oletusnimi tai sähköpostikansioon nimi. Jätä liittäminen päivämäärän käyttöönoton otsikko-valintaruutu on valittuna.

    ![Ohjatun julkaisutoiminnon kolmannessa näytössä](./media/vs-azure-tools-publish-azure-application-wizard/IC749014.png)

1. Valitse **tallennustilan** Asiakasluettelo tallennustilan-tili, jota käytetään tätä käyttöönottoa varten. Vertaa pilvipalvelussa sijaitsevaan ja tallennustilaa-tilisi tiedot-keskukset sijainnit. Ihannetapauksessa paikoista on oltava sama.

    >[AZURE.NOTE] Azure-tallennustilan tilin tallentaa sovelluksen käyttöönottoa pakkaaminen. Kun sovellus on otettu käyttöön, paketti poistetaan tallennustilan tilin.

1. Valitse **käyttöönoton Päivitä** -valintaruutu, jos haluat ottaa käyttöön vain päivitettyjä osia. Tällaista käyttöönotto voi olla nopeampaa kuin koko käyttöönotto. Valitse **asetukset** -linkki avaa **käyttöönoton asetusten päivittäminen** -valintaikkuna, seuraavassa kuvassa. 

    ![Käyttöönottoasetukset](./media/vs-azure-tools-publish-azure-application-wizard/IC617060.png)

    Voit valita jommankumman kahdella eri tavalla päivityksen käyttöönoton vaiheittainen tai samanaikaisesti. Vaiheittainen käyttöönoton päivittää yhden käyttöön esiintymän kerrallaan, niin, että sovelluksesi pysyy online ja käyttäjien käytettävissä. Samanaikainen käyttöönoton päivittää kaikki käyttöön esiintymät yhdellä kertaa. Samanaikaisen päivityksen on nopeampaa kuin lisäävän päivityksen, mutta jos valitset tämän vaihtoehdon, sovelluksen eivät välttämättä ole käytettävissä päivityksen aikana.

    Valitse valintaruutu, jos käyttöönoton ei voi päivittää, tehdä koko käyttöönottoa, jos haluat, että koko käyttöönottoa tapahtuu automaattisesti Jos käyttöönoton päivitys epäonnistuu. Koko käyttöönottoa palauttaa pilvipalvelussa virtual IP (VIP)-osoite. Lisätietoja on artikkelissa [Toimintaohje: säilyttää vakion Virtual IP-osoite pilvipalveluun](https://msdn.microsoft.com/library/azure/jj614593.aspx).


1. Virheenkorjaus palvelun **Käyttöön IntelliTrace** valintaruutu, tai jos otetaan käyttöön **Virheenkorjaus** määritys ja haluat korjata oman pilvipalvelussa Azure-tietokannassa, valitse remote muistin palvelut otetaan käyttöön **Ottaminen käyttöön Remote virheenkorjaus kaikki roolit** -valintaruutu.

2. Voit profiilin sovelluksen **profiloinnin** -valintaruutu ja valitsemalla sitten **asetukset** -linkki näyttää profiloinnin. 


    >[AZURE.NOTE] On käytettävä Visual Studio Ultimate IntelliTrace ja taso-vuorovaikutus profilointi (TIP) ottaminen käyttöön ja et voi ottaa käyttöön molemmat samanaikaisesti.

    Lisätietoja on artikkelissa [Virheenkorjaus julkaistu pilvipalveluun IntelliTrace ja Visual Studio](https://msdn.microsoft.com/library/azure/ff683671.aspx) ja [testaaminen pilvipalveluun suorituskykyä](https://msdn.microsoft.com/library/azure/hh369930.aspx).

1. Valitse **Seuraava** sovelluksen yhteenveto-sivua.

## <a name="publishing-your-application"></a>Sovelluksen julkaiseminen

1. Voit luoda julkaisun profiilin asetuksista, jotka olet valinnut. Voit esimerkiksi luoda testiympäristössä oletusprofiiliksi ja toinen tuotannon. Jos haluat tallentaa profiilia, valitse **Tallenna** -kuvaketta. Ohjattu toiminto luo profiilin ja tallentaa sen Visual Studio Projectissa. Jos haluat muokata profiilinimi, Avaa **kohteen profiili** -luettelo ja valitse sitten **<... hallinta >**.

    ![Ohjatun julkaisutoiminnon yhteenveto näyttö](./media/vs-azure-tools-publish-azure-application-wizard/IC749015.png)

    >[AZURE.NOTE] Julkaisun profiilin näkyy ratkaisunhallinnassa Visual Studiossa ja profiiliasetukset kirjoitetaan tiedostoon .azurePubxml-tunniste. Asetukset tallennetaan XML-tunnisteita määritteet.

1. Valitse **Julkaise** , jos haluat julkaista sovelluksesi. Voit valvoa prosessin tila Visual Studiossa **kohde** -ikkunassa.

## <a name="see-also"></a>Katso myös

[Toimintaohje: Siirrä ja julkaista Web-sovelluksen Azure pilvipalveluun Visual Studio](https://msdn.microsoft.com/library/azure/hh420322.aspx)

[Azure-työkaluilla pilvipalveluun julkaiseminen](https://msdn.microsoft.com/library/azure/ff683672.aspx)

[Virheenkorjaus julkaistun pilvipalvelussa IntelliTrace ja Visual Studio](https://msdn.microsoft.com/library/azure/ff683671.aspx)

[Pilvipalveluun suorituskyvyn testaaminen](https://msdn.microsoft.com/library/azure/hh369930.aspx)

