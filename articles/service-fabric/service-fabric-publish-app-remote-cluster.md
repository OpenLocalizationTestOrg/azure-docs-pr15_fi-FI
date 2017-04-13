<properties
    pageTitle="Sovelluksen julkaiseminen remote klusterin Visual Studiossa | Microsoft Azure"
    description="Katso lisätietoja sovelluksen remote palvelun kangasta klusteriin julkaiseminen Visual Studiossa."
    services="service-fabric"
    documentationCenter="na"
    authors="cawams"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="07/29/2016"
    ms.author="cawa" />

# <a name="publish-an-application-to-a-remote-cluster-by-using-visual-studio"></a>Sovelluksen remote klusteriin julkaiseminen Visual Studion avulla

> [AZURE.SELECTOR]
- [PowerShellin](service-fabric-deploy-remove-applications.md)
- [Visual Studio](service-fabric-publish-app-remote-cluster.md)

<br/>

Visual Studio Azure palvelun kangasta-laajennus on helppoa, toistettavien ja scriptable valinnaiseksi palvelun kangasta klusteriin tapa.

## <a name="the-artifacts-required-for-publishing"></a>Pakollinen julkaisun palvelutiedot

### <a name="deploy-fabricapplicationps1"></a>Ottaa käyttöön FabricApplication.ps1

Tämä on PowerShell-komentosarja, joka käyttää Julkaise profiilin polku parametrina julkaisun palvelun kangasta sovellukset. Tämä komentosarja on sovelluksen osa, olet Tervetuloa Muokkaa sovelluksen tarpeen mukaan.

### <a name="publish-profiles"></a>Julkaise-profiileista

Palvelun kangasta-sovelluksen projektin nimi **PublishProfiles** kansion sisältää XML-tiedostot, joihin voidaan tallentaa keskeiset tiedot julkaisemisen sovellus, kuten:

- Palvelun kangasta klusterin yhteyden parametrit
- Sovelluksen-parametrin tiedoston polku
- Päivitysasetukset

Oletusarvon mukaan sovelluksen sisältää kaksi julkaista profiilit: Local.xml ja Cloud.xml. Voit lisätä profiilien kopioimalla ja liittämällä jokin oletusarvo-tiedostoista.

### <a name="application-parameter-files"></a>Sovelluksen parametritiedostot

Palvelun kangasta-sovelluksen projektin nimi **ApplicationParameters** kansion sisältää käyttäjän määrittämän sovelluksen tiedostojen parametriarvot XML-tiedostoja. Sovelluksen tiedostojen tiedostot voi Parametroitu niin, että voit käyttää eri arvoja Käyttöönottoasetukset. Saat lisätietoja sovelluksen parametrointi on [useita ympäristöissä-palvelun kangasta hallinta](service-fabric-manage-multiple-environment-app-configuration.md).

>[AZURE.NOTE] Toimija-palveluja varten sinun on muodostettava projektin ensin ennen kuin yrität muokata tiedostoa muokkaajan tai Julkaise-valintaikkunan kautta. Tämä johtuu siitä osa luettelon tiedostot luodaan Luo aikana.

## <a name="to-publish-an-application-by-using-the-publish-service-fabric-application-dialog-box"></a>Valinnaiseksi julkaista kangasta palvelusovelluksen-valintaikkunan avulla

Seuraavat vaiheet näytetään, miten valinnaiseksi Visual Studio palvelun kangasta Tools myöntämä **Julkaista kangasta palvelusovelluksen** -valintaikkunan avulla.

1. Valitse pikavalikosta kangasta palvelusovelluksen projektin **Julkaise...** Voit tarkastella **Julkaista kangasta palvelusovelluksen** -valintaikkunassa.

    ![** Julkaista palvelun kangasta sovelluksen **-valintaikkuna][0]

    Valitun **kohteen profiili** -avattavassa luettelossa tiedoston on kaikki asetukset, paitsi **luettelon versiot**, tallennuspaikan. Voit käyttää aiemmin luotuun profiiliin uudelleen tai luoda uuden valitsemalla **<... hallinta profiilit >** **kohteen profiili** -avattavassa luettelossa. Kun valitset Julkaise profiili, sen sisältö näkyy vastaavien kenttien-valintaikkunan. Tallenna muutokset milloin tahansa, valitse **Tallenna profiili** -linkki.    

2. Määritä paikallisessa tai etätietokoneessa palvelun kangasta klusterin 's julkaisun päätepisteen **yhteyden endpoint** -osassa. Voit lisätä tai muuttaa yhteyden endpoint, napsauttamalla **Yhteyden Endpoint** avattavasta luettelosta. Luettelossa näkyvät käytettävissä olevat palvelun kangasta klusterin yhteyden päätepisteet, johon voit julkaista Azure tilauksistasi perusteella. Huomaa, että jos et ole jo kirjautunut ja Visual Studio, voit pyydetään tekemään niin.

    Klusterin valinta-valintaikkunan avulla voit valita käytettävissä tilaukset ja klustereiden.

    ![** Valitse palvelu kangasta klusterin **-valintaikkuna][1]

    >[AZURE.NOTE] Jos haluat julkaista haluamaansa päätepisteen (kuten osapuolen klusterin)-kohdassa **julkaisun haluamaansa klusterin päätepisteen** on kuvattu.

    Kun valitset päätepisteen, Visual Studio vahvistaa valitun palvelun kangasta klusterin yhteys. Jos klusterin ei ole suojattu, Visual Studio muodostaa siihen välittömästi. Kuitenkin jos klusterin on suojattu, sinun on varmenteen asentaminen paikalliseen tietokoneeseen ennen kuin jatkat. Lisätietoja on kohdassa [määrittäminen suojattuja yhteyksiä](service-fabric-visualstudio-configure-secure-connections.md) . Kun olet valmis, valitse **OK** -painiketta. Valitun klusterin näkyy **Julkaista kangasta palvelusovelluksen** -valintaikkunassa.

3. Siirry **Sovelluksen parametri tiedosto** avattavassa luettelossa sovelluksen-parametri tiedosto. Äänitiedoston sovelluksen parametri on käyttäjän määrittämä arvoja parametrien sovelluksen tiedostojen tiedostossa. Voit lisätä tai muuttaa parametrin, valitse **Muokkaa** -painiketta. Määritä tai muuta parametrin arvon **Parametrit** -ruudukossa. Kun olet valmis, valitse **Tallenna** -painiketta.

    ![** Parametrit Muokkaa **-valintaikkuna][2]

4. **Päivitä sovellus** -valintaruutu, jos haluat määrittää, onko tämä julkaista toiminto on päivitystä. Päivityksen julkaista toiminnot Normaali poiketa julkaista toiminnot. Katso erot luettelo [Palvelun kangasta sovelluksen päivittäminen](service-fabric-application-upgrade.md) . Voit määrittää päivitysasetukset, valitsemalla **Määritä päivityksen asetukset** -linkki. Päivityksen Parametrieditori tulee näkyviin. Katso lisätietoja päivityksen parametrien [määrittäminen palvelun kangasta sovelluksen päivitys](service-fabric-visualstudio-configure-upgrade.md) .

5. Valitse **luettelon versiot...** **Muokkaa versiot** -valintaikkuna-painiketta. Sinun täytyy päivittää sovelluksen ja palvelun versio on päivitettävä asetetaan. Artikkelissa kerrotaan, miten sovellus- ja palveluluettelon luettelon versiot vaikutus päivitysprosessin [palvelun kangasta sovelluksen päivittäminen opetusohjelma](service-fabric-application-upgrade-tutorial.md) .

    ![** Versiot Muokkaa **-valintaikkuna][3]

    Jos sovelluksen ja palvelun versioita käyttävät 1.0.0.0 muoto semanttinen versiotiedot, kuten 1.0.0 tai numeerisia arvoja, valitse **Päivitä automaattisesti sovelluksen ja palvelun versiot** -vaihtoehto. Kun valitset tämän vaihtoehdon, palvelu ja sovelluksen versio numerot päivitetään automaattisesti aina, kun koodi, config tai tietojen pakkaaminen versio on päivitetty. Jos haluat muokata versiot manuaalisesti, poista valintaruudun valinta poistaa tämän toiminnon käytöstä.

    >[AZURE.NOTE] Toimija-projektin näkyvän kaikkien paketin merkintöjen luominen projektin luomiseen tapahtumat palvelun luettelon tiedostot.

6. Kun olet valmis määrittämällä kaikki tarvittavat asetukset, valitse **Julkaise** -painikkeen voit julkaista valitun palvelun kangasta klusterin sovellusta. Julkaisuprosessin kohdistetaan, jonka olet määrittänyt asetukset.

## <a name="publish-to-an-arbitrary-cluster-endpoint-including-party-clusters"></a>(Mukaan lukien osapuolen klustereiden) haluamaansa klusterin päätepisteen julkaiseminen

Visual Studio julkaiseminen kokemus on tarkoitettu remote klustereiden jokin Azure tilauksista liittyvät julkaiseminen. On kuitenkin mahdollista Julkaise haluamaansa päätepisteet (kuten palvelun kangasta osapuolen klustereiden) muokkaamalla suoraan Julkaise profiilin XML. Yllä olevien ohjeiden mukaisesti kaksi julkaista profiilit annetaan oletusarvoinen--**Local.xml** ja **Cloud.xml**--, mutta et Tervetuloa luoda lisää profiileja eri ympäristöissä. Haluat esimerkiksi luoda julkaisun osapuolen klustereihin nimeltä ehkäpä **Party.xml**profiilin.

Jos olet muodostamassa yhteyttä suojaamaton klusterin, kaikki haluamasi on pakollinen on klusterin yhteyden endpoint, kuten `partycluster1.eastus.cloudapp.azure.com:19000`. Tässä tapauksessa yhteyden endpoint Julkaise profiilin näyttää suunnilleen tältä:

```XML
<ClusterConnectionParameters ConnectionEndpoint="partycluster1.eastus.cloudapp.azure.com:19000" />
```

  Yrität muodostaa yhteyden suojattuun klusteriin, sinun on myös paikallisen kaupasta käytettävän todennustavaksi Asiakasvarmenne tiedot. Lisätietoja on artikkelissa [määrittäminen suojattua yhteyttä palvelun kangasta klusteriin](service-fabric-visualstudio-configure-secure-connections.md).

  Kun Julkaise profiilin on määritetty, voit viitata sen Julkaise-valintaikkunassa alla kuvatulla tavalla.

  ![Julkaise uusi profiili-Julkaise-valintaikkuna][4]

  Huomaa, että tässä tapauksessa Julkaise uusi profiili viittaa jonkin oletusarvon sovelluksen parametrin tiedostojen. Käytä tätä vaihtoehtoa, jos haluat julkaista sovelluksen määrityksen mukaisesti ympäristöissä määrään. Sen sijaan haluamaasi on eri määritykset kussakin ympäristössä, jonka haluat julkaista tapauksissa se tulkita vastaavan sovelluksen parametri-tiedoston luominen.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja siitä, miten voit automatisoida julkaisuprosessin ympäristössä, jossa on jatkuva integrointi on artikkelissa [palvelun kangasta jatkuva integrointi määrittäminen](service-fabric-set-up-continuous-integration.md).


[0]: ./media/service-fabric-publish-app-remote-cluster/PublishDialog.png
[1]: ./media/service-fabric-publish-app-remote-cluster/SelectCluster.png
[2]: ./media/service-fabric-publish-app-remote-cluster/EditParams.png
[3]: ./media/service-fabric-publish-app-remote-cluster/EditVersions.png
[4]: ./media/service-fabric-publish-app-remote-cluster/publish-to-party-cluster.png
