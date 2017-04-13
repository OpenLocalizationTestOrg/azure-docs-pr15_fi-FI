<properties 
pageTitle="Etätyöpöytäyhteys – Azure pilvipalveluihin roolia ottaminen käyttöön" 
description="Azure cloud palvelusovelluksen sallimaan työpöydän etäyhteyksien määrittäminen" 
services="cloud-services" 
documentationCenter="" 
authors="sbtron" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="02/17/2016" 
ms.author="saurabh"/>

# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Etätyöpöytäyhteys – Azure pilvipalveluihin roolia ottaminen käyttöön

>[AZURE.SELECTOR]
- [Azure perinteinen portal](cloud-services-role-enable-remote-desktop.md)
- [PowerShellin](cloud-services-role-enable-remote-desktop-powershell.md)
- [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)


Etätyöpöytä mahdollistaa käynnissä Azure rooli työpöydältä. Etätyöpöytäyhteys voit sovelluksen ongelmien vianmääritystä on käynnissä varten. 

Voit ottaa Etätyöpöytäyhteys käyttöön tehtäväsi kehityksen aikana sisällyttämällä etätyöpöydän moduulit palvelun määrityksessä tai voit ottaa käyttöön etätyöpöydän kautta Remote Desktop-tunniste. Ensisijainen tapa on käyttää etätyöpöydän tunniste voit ottaa etätyöpöydän senkin jälkeen, kun sovellus otetaan käyttöön eikä sinun tarvitse Ota sovelluksesi uudelleen. 


## <a name="configure-remote-desktop-from-the-azure-classic-portal"></a>Määritä etätyöpöydän perinteinen Azure-portaalista
Azure perinteinen portaalin käyttää Remote Desktop-tunniste lähestymistapa, joten voit ottaa etätyöpöydän senkin jälkeen, kun sovellus otetaan käyttöön. Cloud-palvelun **määrittäminen** -sivulla voit ottaa käyttöön etätyöpöydän kautta, muuttaa paikallisen järjestelmänvalvojatilin avulla muodostetaan yhteys näennäiskoneiden varmenteen todennuksessa käytetyn, vanhenemispäivämäärän asettaminen. 


1. Valitse **Pilvipalveluihin**, nimeä pilvipalvelussa, ja valitse sitten **Määritä**.

2. Valitse **Remote**.
    
    ![Remote pilvipalveluihin](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)
    
    > [AZURE.WARNING] Kaikki roolin esiintymät käynnistetään, kun ensin etätyöpöydän ottaminen käyttöön ja valitse OK (valintamerkki). Jos haluat estää uudelleenkäynnistyksen, salasana salaamisessa käytettyä varmennetta on oltava asennettuna rooli. Voit estää uudelleen, [Lataa varmenteen pilvipalvelussa](cloud-services-how-to-create-deploy/#how-to-upload-a-certificate-for-a-cloud-service) ja palaa sitten tässä valintaikkunassa.
    

3. **Roolit**Valitse Päivitä tai valitse **kaikki** rooleista rooli.

4. Tee seuraavat muutokset:
    
    - Etätyöpöytä käyttöön valitsemalla **Ota käyttöön etätyöpöytä** -valintaruutu. Voit poistaa käytöstä etätyöpöydän kautta, poista valintaruudun valinta.
    
    - Luo tili käyttäminen etätyöpöydän yhteyksien rooli esiintymät.
    
    - Päivitä aiemmin tilin salasanan.
    
    - Valitse ladatut varmenne, joka käyttää todennus (Lataa käyttämällä **Varmenteet** -sivulla **Lataa** sertifikaatti) tai Luo uusi varmenne. 
    
    - Etätyöpöytä määritykset päättymispäivän muuttaminen

5. Kun määritykset Päivityksesi on valmis, valitse **OK** (valintamerkki).


## <a name="remote-into-role-instances"></a>Etäyhteyksien roolin esiintymät tuominen
Kun Etätyöpöytä on otettu käyttöön rooleille voit etäyhteyksien rooli-esiintymän eri työkaluja kautta.

Voit yhdistää roolin esiintymän perinteinen Azure-portaalista:
    
  1.   Valitse Avaa sivun **tapaukset** **esiintymät** .
  2.   Valitse rooli-esiintymä, joka on määritetty Etätyöpöytä.
  3.   Valitse **Yhdistä**ja noudata ohjeita avataksesi työpöydälle. 
  4.   Valitse **Avaa** ja valitse **Yhdistä** Etätyöpöytäyhteys Aloita. 


### <a name="use-visual-studio-to-remote-into-a-role-instance"></a>Käyttää Visual Studio remote kyselyjä rooli-esiintymä

Visual Studiossa, palvelimen Explorer:

1. Laajenna **Azure\\pilvipalveluihin\\[cloud palvelunimi]** solmu.
2. Laajenna **väliaikaisen** tai **tuotannon**.
3. Laajenna yksittäisiä rooli.
4. Napsauta hiiren kakkospainikkeella jotakin roolin esiintymät, valitse **Yhdistä käyttäen etätyöpöydän...**ja Anna käyttäjänimi ja salasana. 

![Explorer Etätyöpöytä](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)


### <a name="use-powershell-to-get-the-rdp-file"></a>Hae RDP-tiedoston PowerShellin avulla
[Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) cmdlet-komennon avulla voit hakea RDP-tiedostoa. Voit sitten käyttää pilvipalvelussa sijaitsevaan Etätyöpöytäyhteys kanssa RDP-tiedoston avulla.

### <a name="programmatically-download-the-rdp-file-through-the-service-management-rest-api"></a>Lataa ohjelmallisesti Service Management REST-Ohjelmointirajapinnalla kautta RDP-tiedosto
Voit ladata RDP-tiedoston [RDP-tiedoston lataaminen](https://msdn.microsoft.com/library/jj157183.aspx) REST-toiminnon avulla. 



## <a name="to-configure-remote-desktop-in-the-service-definition-file"></a>Etätyöpöytä paikallaan määritelmä-palvelutiedosto

Tätä menetelmää voit etätyöpöydän käyttöön sovellusten kehitystyön aikana. Tämän menetelmän edellyttää salattua salasanojen tallennetaan palvelun kokoonpanoa tiedoston ja päivitykset remote työpöydän työnkulkuun edellyttäisi lukea sovelluksen. Jos haluat välttää nämä downsides käytettävä kuvattuun remote työpöydän tunnisteen mukaan-vaihtoehto.  

Voit käyttää Visual Studio [käyttöön Etätyöpöytäyhteys](../vs-azure-tools-remote-desktop-roles.md) palvelun määritelmä tiedoston menetelmän avulla.  
Seuraavia ohjeita kuvaavat service-mallitiedostot etätyöpöydän käyttöön tarvittavat muutokset. Visual Studio automaattisesti tekee nämä muutokset, kun julkaiset.

### <a name="set-up-the-connection-in-the-service-model"></a>Mallissa service-yhteyden määrittäminen 
**Tuonti** -osan avulla voit tuoda **RemoteAccess** -moduulin ja **RemoteForwarder** -moduulin [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) -tiedostoon.

Palvelun määritystiedoston pitäisi olla samankaltainen seuraavan esimerkin kanssa `<Imports>` lisätty elementti.

```xml
<ServiceDefinition name="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2013-03.2.0">
    <WebRole name="WebRole1" vmsize="Small">
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="Endpoint1" endpointName="Endpoint1" />
                </Bindings>
            </Site>
        </Sites>
        <Endpoints>
            <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
            <Import moduleName="Diagnostics" />
            <Import moduleName="RemoteAccess" />
            <Import moduleName="RemoteForwarder" />
        </Imports>
    </WebRole>
</ServiceDefinition>
```
[ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) tiedoston pitäisi olla samankaltainen kuin seuraavassa esimerkissä, Huomaa `<ConfigurationSettings>` ja `<Certificates>` osat. Määritetty varmenne on oltava [ladattu pilvipalveluun](../cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2013-03.2.0">
    <Role name="WebRole1">
        <Instances count="2" />
        <ConfigurationSettings>
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="[name-of-user-account]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="[base-64-encrypted-user-password]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="[certificate-expiration]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" />
        </ConfigurationSettings>
        <Certificates>
            <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="[certificate-thumbprint]" thumbprintAlgorithm="sha1" />
        </Certificates>
    </Role>
</ServiceConfiguration>
```


## <a name="additional-resources"></a>Lisäresursseja

[Pilvipalveluihin määrittäminen](cloud-services-how-to-configure.md)