<properties 
    pageTitle="Luominen Azure Resurssienhallinta-mallien avulla ILB-Ietokannan | Microsoft Azure" 
    description="Opettele luomaan sisäinen kuormituksen tasauspalvelun Ietokannan Azure Resurssienhallinta mallien avulla." 
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
    ms.date="09/21/2016" 
    ms.author="stefsch"/>   

# <a name="how-to-create-an-ilb-ase-using-azure-resource-manager-templates"></a>Azure Resurssienhallinta-mallien avulla ILB-Ietokannan luominen

## <a name="overview"></a>Yleiskatsaus ##
Sovelluksen palvelun ympäristöissä voi luoda VPN sisäinen osoitteessa julkisen VIP sijaan.  Sisäisen osoitteen annetaan Azure-komponentin kutsutaan sisäinen kuormituksen (ILB).  ILB Ietokannan voi luoda Azure-portaalissa.  Voit myös luoda automaatio kautta Azure Resurssienhallinta mallien avulla.  Tässä artikkelissa käydään läpi vaiheet ja syntaksi luomiseksi ILB Ietokannan Azure Resurssienhallinta malleja.

Yhdistävää automatisointi ILB Ietokannan luominen on kolme vaihetta:
1. Perus Ietokannan luodaan ensin virtual verkon sisäinen kuormituksen tasauspalvelun osoitetta käyttämällä julkisen VIP sijaan.  Kun tämä vaihe, ensisijaisen toimialuenimen on määritetty ILB Ietokannan.
2. Kun ILB Ietokannan on luotu, SSL-varmenne on ladattu palvelimeen.  
3. Ladatun SSL-varmenteen määritetään erikseen ILB Ietokannan kuin sen "oletus" SSL-varmenne.  SSL-varmenteessa käytetään SSL-liikenteen ILB Ietokannan-sovellukset, kun sovellukset on osoitettu yleisiä päätoimialue myönnetyt Ietokannan (kuten https://someapp.mycustomrootcomain.com)

## <a name="creating-the-base-ilb-ase"></a>Base ILB Ietokannan luominen ##
Esimerkki Azure Resurssienhallinta-mallin toista ja sen liittyviä parametreja-tiedostona, ovat käytettävissä GitHub [tähän][quickstartilbasecreate].

Parametrit- *azuredeploy.parameters.json* tiedostossa on yleisiä luomiseen sekä ILB ASEs sekä ASEs sidottu julkisen VIP.  Puhelut, erityinen huomautus parametrit alla olevassa luettelossa tai jotka ovat yksilöllisiä, ILB Ietokannan luotaessa:


- *interalLoadBalancingMode*: useimmissa tapauksissa tämä 3, mikä tarkoittaa sekä HTTP/HTTPS tietoliikenteen portit 80 ja 443 ja valvontatiedot/kanavan portit kuunteli Ietokannan, FTP-palvelun määrittäminen sitoa ILB kohdistettu sisäinen virtual verkko-osoite.  Jos tämä ominaisuus on sen sijaan arvoksi 2, FTP-palvelun liittyvät portit (ohjausobjektia ja tietoja-kanavat) sidottava ILB osoite, kun HTTP/HTTPS-liikenne säilyy julkisen VIP.
-  *dnsSuffix*: Tämä parametri määrittää oletusarvon päätoimialue, joka määritetään Ietokannan.  Azure-sovelluksen-palvelun julkisen variaation kaikki verkkosovelluksissa pääkansion oletustoimialueen on *azurewebsites.net*.  Kuitenkin ILB Ietokannan on sisäinen asiakkaan virtual verkkoon, ei kannata käyttää julkisen palvelun oletusarvon päätoimialue.  Sen sijaan ILB Ietokannan pitäisi olla pääkansio oletustoimialueen, joka on järkevää yrityksen sisäinen virtual verkossa oleville käyttöä varten.  Esimerkiksi hypoteettista Contoso Corporation voi käyttää oletusarvo-päätoimialue, *sisäinen contoso.com* sovellukset, jotka on tarkoitettu vain on ratkaistava ja käytettävissä Contoson virtual verkossa. 
-  *ipSslAddressCount*: Tämä parametri automaattisesti oletusarvo arvo on 0, *azuredeploy.json* -tiedostossa, koska ILB ASEs on vain yksi ILB-osoite.  Ei ole eksplisiittinen IP-SSL-osoitteita ILB-Ietokannan, ja näin ollen ILB Ietokannan IP-SSL-osoitteen resurssivarantoon määrityksen on oltava nolla, muuten valmistelu virhe ilmenee. 

Kun *azuredeploy.parameters.json* -tiedosto on täytetty ILB Ietokannan varten, voit ILB Ietokannan sitten luotu Powershell koodikatkelman.  Voit muuttaa tiedoston polut vastaamaan missä Azure Resurssienhallinta-mallitiedostot sijaitsevat käyttämääsi laitteeseen.  Muista myös omia arvot Azure Resurssienhallinta käyttöönoton nimen ja resurssiryhmän nimi.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"
    
    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Kun Azure Resurssienhallinta malli lähetetään kestää muutaman tunnin ILB Ietokannan luoda varten.  Kun luominen on valmis, ILB Ietokannan näkyy luettelossa App palvelun ympäristöissä tilaukseen, joka on käynnistänyt käyttöönoton UX-portaalissa.

## <a name="uploading-and-configuring-the-default-ssl-certificate"></a>Lataaminen ja "Oletus" SSL-varmenteen määrittäminen ##

Kun ILB Ietokannan on luotu, SSL-varmenteen on oltava liity Ietokannan, "oletus" SSL-varmenteen Käytä SSL-yhteyksien sovelluksiin määrittämiseksi.  Jos Ietokannan oletus DNS hypoteettista Contoso Corporation olevassa esimerkissä jatkamista Suffiksi *sisäinen contoso.com*ja valitse *https://some-random-app.internal-contoso.com* yhteyden edellyttää SSL-varmenne, joka on voimassa **.internal contoso.com*. 

Useilla eri tavoilla voit hankkia kelvollinen SSL-varmenteen mukaan lukien sisäinen CAs ja olet ostanut varmenteen ulkoisen myöntäjä käyttäminen itse allekirjoitettua varmennetta.  SSL-varmenteen lähteen, vaikka varmenteen määritteet on määritetty oikein:

- *Aihe*: tämän määritteen on määritettävä **.your-pääkansio-toimialue-here.com*
- *Aiheen vaihtoehtoinen nimi*: tämän määritteen on oltava sekä * *.your-pääkansio-toimialue-here.com*ja * *.scm.your-pääkansio-toimialue-here.com*.  Toinen merkintä syy on, että kunkin sovelluksen liittyvän palvelujen ohjauksen hallinta/Kudu sivuston SSL-yhteyksiä tehdään osoitetta-lomakkeen *your-app-name.scm.your-root-domain-here.com*käyttämällä.

Käsi kelvollinen SSL-varmenne, jossa kaksi valmistelevat lisävaiheita tarvitaan.  SSL-varmenne on oltava muuntaa/tallennettu .pfx-tiedostona.  Muista, että .pfx-tiedosto on sisällyttää kaikki keskitason ja pääkansio varmenteet ja myös on suojattu salasanalla.

Valitse Seuraava .pfx-tiedosto on muunnetaan base64 merkkijonon, koska SSL-varmenteen ladataan Azure Resurssienhallinta-mallin avulla.  Koska Azure Resurssienhallinta mallit ovat tekstitiedostoista, .pfx-tiedosto on muunnetaan base64 merkkijonon, jotta se voidaan sisällyttää parametrina mallin.

PowerShellin koodikatkelman alla näkyy esimerkki luodaan itse allekirjoitettua varmennetta, varmenteen vieminen .pfx-tiedostona .pfx-tiedoston muuntaminen base64-koodatun merkkijonon ja tallentamalla base64-koodatun merkkijonon erilliseen tiedostoon.  Base64 koodaustoiminnon Powershell-koodi on toimitettu [Powershell-komentosarjojen blogi][examplebase64encoding].

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.pfx"
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     
    
    $fileContentBytes = get-content -encoding byte $fileName
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    $fileContentEncoded | set-content ($fileName + ".b64")
    
Kun SSL-varmenne on onnistuneesti luonut ja koodattu base64 muunnetaan merkkijono, esimerkiksi Azure Resurssienhallinta mallia, GitHub, [oletusarvon SSL-varmenteen määrittäminen] [ configuringDefaultSSLCertificate] voidaan käyttää.

Parametrit- *azuredeploy.parameters.json* tiedostossa on lueteltu alla:

- *appServiceEnvironmentName*: on määritetty ILB Ietokannan nimi.
- *existingAseLocation*: tekstimerkkijono, joka sisältää Azure alue, jossa ILB Ietokannan on otettu käyttöön.  Esimerkiksi: "Etelä keskitetyn US".
- *pfxBlobString*: based64 koodatun merkkijonon esittäminen .pfx-tiedosto.  Käytä esitettyjä koodikatkelman, "exportedcert.pfx.b64" sisältää merkkijonon kopioida ja liittää sen *pfxBlobString* -määritteen arvon.
- *salasana*: salasana suojaa .pfx-tiedosto.
- *certificateThumbprint*: varmenteen allekirjoitus.  Jos tämä arvo noutaa PowerShellin (kuten *$certificate. Allekirjoitus* -aiemmissa koodikatkelman), voit käyttää arvon kuin – on.  Kuitenkin jos kopioit arvon Windows varmenne-valintaikkuna, muista poistaa ylimääräiset välilyönnit.  *CertificateThumbprint* pitäisi näyttää suunnilleen: AF3143EB61D43F6727842115BB7F17BBCECAECAE
- *certificateName*: helpossa muodossa merkkijono-tunnus, valitsemalla Oma käytettävä tunnistetietojen varmenne.  Nimeä käytetään osana Azure Resurssienhallinta yksilöllinen *Microsoft.Web/certificates* kohteen, joka edustaa SSL-varmenne.  Nimen **on oltava** loppuun seuraavat jälkiliitteellä: \_yourASENameHere_InternalLoadBalancingASE.  Tämä jälkiliite käyttämä portaalin varmennetta käytetään suojaaminen ILB käyttävä Ietokannan ilmaisin.


Esimerkki lyhennetty *azuredeploy.parameters.json* on alla:


    {
         "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json",
         "contentVersion": "1.0.0.0",
         "parameters": {
              "appServiceEnvironmentName": {
                   "value": "yourASENameHere"
              },
              "existingAseLocation": {
                   "value": "East US 2"
              },
              "pfxBlobString": {
                   "value": "MIIKcAIBAz...snip...snip...pkCAgfQ"
              },
              "password": {
                   "value": "PASSWORDGOESHERE"
              },
              "certificateThumbprint": {
                   "value": "AF3143EB61D43F6727842115BB7F17BBCECAECAE"
              },
              "certificateName": {
                   "value": "DefaultCertificateFor_yourASENameHere_InternalLoadBalancingASE"
              }
         }
    }

Kun *azuredeploy.parameters.json* -tiedosto on täytetty, oletusarvo SSL-varmenteen voi määrittää käyttämällä Powershell koodikatkelman.  Voit muuttaa tiedoston polut vastaamaan missä Azure Resurssienhallinta-mallitiedostot sijaitsevat käyttämääsi laitteeseen.  Muista myös omia arvot Azure Resurssienhallinta käyttöönoton nimen ja resurssiryhmän nimi.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"
    
    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Kun Azure Resurssienhallinta-malli on lähetetty kestää noin 40 minuuttia minuutit Ietokannan edusta Ota muutokset käyttöön.  Esimerkiksi oletusarvon esitystapa Ietokannan, käyttämällä kahta eteen-päättyy, jossa malli noin yksi tunti ja 20 minuutin ajan suorittaminen kestää.  Malli on käynnissä Ietokannan ei voi skaalata.  

Kun malli on valmis, sovellusten ILB Ietokannan niitä voi käyttää HTTPS-protokollan välityksellä ja yhteydet suojataan oletusarvon SSL-varmenne.  Oletusarvon SSL-varmennetta käytetään, kun ILB Ietokannan-sovellukset on osoitettu sovelluksen nimi sekä oletusarvoinen isäntänimi yhdistelmä.  Esimerkiksi *https://mycustomapp.internal-contoso.com* käyttää oletusarvon SSL-varmennetta **.internal contoso.com*.

Kuitenkin julkisen usean vuokraajan-palvelu käytössä-sovelluksia, kuten kehittäjät voit myös määrittää mukautetun isäntänimet yksittäisten sovellusten ja määritä sitten yksilöllinen SNI SSL varmenteen sidontojen yksittäisten sovellusten.  


## <a name="getting-started"></a>Käytön aloittaminen

Aloita sovelluksen palvelun ympäristöissä artikkelissa [Johdanto App Service-ympäristöön](app-service-app-service-environment-intro.md)

Kaikki artikkelit ja miten-, Muokkaa sovelluksen palvelun ympäristöissä ovat käytettävissä [sovelluksen palvelun ympäristöissä Lueminut-tiedosto](../app-service/app-service-app-service-environments-readme.md).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[quickstartilbasecreate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-create/
[examplebase64encoding]: http://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/ 
 
