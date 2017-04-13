<properties
    pageTitle="Mukautetut asetukset App Service-ympäristössä"
    description="Mukautettujen Hakumääritysten asetusten App Service-ympäristössä"
    services="app-service"
    documentationCenter=""
    authors="stefsch"
    manager="nirma"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="stefsch"/>

# <a name="custom-configuration-settings-for-app-service-environments"></a>Mukautettujen Hakumääritysten asetusten App Service-ympäristössä

## <a name="overview"></a>Yleiskatsaus ##
Koska yhdelle asiakkaalle eristetään App Service-ympäristössä, on tietyt asetukset, joita voidaan käyttää yksinomaan App palvelun ympäristöissä. Tässä artikkelissa asiakirjat, jotka ovat käytettävissä sovelluksen palvelun ympäristöissä eri mukautukset.

Jos sinulla ei ole sovelluksen Service-ympäristössä, katso, [miten voit luoda sovelluksen Service-ympäristössä](app-service-web-how-to-create-an-app-service-environment.md).

Voit tallentaa sovelluksen palvelun ympäristön mukautukset käyttämällä matriisin uusi **clusterSettings** -määrite. Tämän määritteen löytyy *hostingEnvironments* Azure Resurssienhallinta kohteen "Ominaisuudet"-sanasto.

Lyhennetty Resurssienhallinta malli-koodikatkelman näkyy **clusterSettings** -määrite:


    "resources": [
    {
       "apiVersion": "2015-08-01",
       "type": "Microsoft.Web/hostingEnvironments",
       "name": ...,
       "location": ...,
       "properties": {
          "clusterSettings": [
             {
                 "name": "nameOfCustomSetting",
                 "value": "valueOfCustomSetting"
             }
          ],
          "workerPools": [ ...],
          etc...
       }
    }

**ClusterSettings** -määrite voidaan lisätä Resurssienhallinta mallin päivittää sovelluksen Service-ympäristössä.

## <a name="use-azure-resource-explorer-to-update-an-app-service-environment"></a>Azure resurssin Resurssienhallinnan avulla voit päivittää sovelluksen Service-ympäristössä
Vaihtoehtoisesti voit päivittää sovelluksen Service-ympäristön [Azure resurssin Resurssienhallinnan](https://resources.azure.com)avulla.  

1. Resurssin Explorerissa Siirry solmu App Service-ympäristössä (**tilaukset** > **resourceGroups** > **tarjoajien** > **Micrososft.Web** > **hostingEnvironments**). Valitse tietyn, jonka haluat päivittää sovelluksen Service-ympäristössä.

2. Valitse oikeanpuoleisessa ruudussa **Luku-/ kirjoitusoikeudet** yläkulmassa työkalurivin Salli vuorovaikutteinen muokkaaminen resurssin Resurssienhallinnassa.  

3. Napsauta sininen **muokata** luoda Resurssienhallinta-mallin voi muokata-painiketta.

4. Vieritä oikeanpuoleisessa ruudussa alaspäin. **ClusterSettings** -määrite on hyvin alaosasta, johon voit kirjoittaa tai päivittää sen arvon.

5. Kirjoita (tai kopioi ja liitä) määritysten arvojen haluat **clusterSettings** määritteeseen matriisi.  

6. Valitse vihreä **valitseminen** painiketta, joka sijaitsee Vahvista muutos App Service-ympäristöön oikeanpuoleisen ruudun yläreunassa.

Lähetä muutokset, mutta kestää kerrottuna eteen päättyy App palvelun ympäristössä, jotta muutos tulee voimaan noin 30 minuuttia.
Jos ympäristössä App palvelu on neljä eteen päättyy, kestää noin kaksi tuntia Viimeistele määritys-päivitys. Määritysten muutos on parhaillaan esiin, kun muita skaalauksen toimintojen tai määritysten muuttaminen toimintoja ei voi kestää paikka App Service-ympäristössä.

## <a name="disable-tls-10"></a>TLS 1.0 poistaminen käytöstä ##
Asiakkaiden toistuvan kysymys, erityisesti asiakkaat, jotka ovat käsittely PCI yhteensopivuuden valvoo, on poistamisesta käytöstä erikseen niiden sovellusten TLS 1.0.

TLS 1.0 voidaan poistaa käytöstä seuraavan **clusterSettings** merkinnän kautta:

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],

## <a name="change-tls-cipher-suite-order"></a>Muuta TLS salaus suite järjestystä ##
Toiseen kysymys asiakkaiden on, jos he voivat muokata neuvottelema niiden palvelimen ciphers luettelo ja tämä onnistuu muokkaamalla **clusterSettings** alla kuvatulla tavalla. Salaus tuotepakettien käytettävissä luettelo voi hakea [tämän MSDN-artikkelista] (https://msdn.microsoft.com/library/windows/desktop/aa374757(v=vs.85\).aspx).

        "clusterSettings": [
            {
                "name": "FrontEndSSLCipherSuiteOrder",
                "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
            }
        ],

> [AZURE.WARNING]  Jos salaus-ohjelmistopaketti, joka SChannel ymmärrä virheellisten arvojen on määritetty, kaikki palvelimeen TLS viestintä saattaa lakata toimimasta. Tässä tapauksessa sinun on *FrontEndSSLCipherSuiteOrder* merkinnän poistaminen **clusterSettings** ja Lähetä päivitetyt Resurssienhallinta malleja salaus suite oletusasetukset palautetaan.  Käytä tätä toimintoa harkiten.

## <a name="get-started"></a>Käytön aloittaminen
Azure pikaopas Resurssienhallinta mallin sivusto sisältää malli, jonka perus määrityksen [luominen sovelluksen Service-ympäristössä](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).


<!-- LINKS -->

<!-- IMAGES -->
