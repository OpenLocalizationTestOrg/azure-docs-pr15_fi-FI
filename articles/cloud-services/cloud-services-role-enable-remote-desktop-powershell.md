<properties
pageTitle="Etätyöpöytäyhteys käyttöön roolia Azure pilvipalveluihin PowerShellin avulla"
description="Salli työpöydän etäyhteyksien PowerShellin avulla azure cloud palvelusovelluksen määrittäminen"
services="cloud-services"
documentationCenter=""
authors="thraka"
manager="timlt"
editor=""/>
<tags
ms.service="cloud-services"
ms.workload="tbd"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="08/05/2016"
ms.author="adegeo"/>

# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a>Etätyöpöytäyhteys käyttöön roolia Azure pilvipalveluihin PowerShellin avulla

>[AZURE.SELECTOR]
- [Azure perinteinen portal](cloud-services-role-enable-remote-desktop.md)
- [PowerShellin](cloud-services-role-enable-remote-desktop-powershell.md)
- [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)


Etätyöpöytä mahdollistaa käynnissä Azure rooli työpöydältä. Etätyöpöytäyhteys voit sovelluksen ongelmien vianmääritystä on käynnissä varten.

Tässä artikkelissa käsitellään käyttöön etätyöpöytä oman Cloud-palvelun roolien PowerShellin avulla. Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) varten tämän artikkelin tarvittavat yhteyden edellytykset ovat olemassa. PowerShell käyttämällä Remote Desktop-tunniste, niin voit ottaa etätyöpöydän, kun sovellus on otettu käyttöön.


## <a name="configure-remote-desktop-from-powershell"></a>Etätyöpöytä PowerShell-asetusten määrittäminen

[Määritä AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) cmdlet-komennon avulla voit etätyöpöydän määritetyn roolien tai kaikki roolit cloud-palvelun käyttöönoton. Cmdlet-komennon avulla voit määrittää käyttäjänimi ja salasana remote työpöydän käyttäjälle, joka hyväksyy PSCredential objektin *tunnistetiedon* parametrin kautta.

Jos käytät PowerShell vuorovaikutteisesti, voit määrittää PSCredential objektin helposti soittamalla [Get-tunnistetiedot](https://technet.microsoft.com/library/hh849815.aspx) cmdlet-komento.

```
$remoteusercredentials = Get-Credential
```

Tämä komento Avaa salliminen syöttää turvallisesti remote käyttäjän käyttäjänimi ja salasana-valintaikkunan.

Koska PowerShellin avulla automation tilanteissa, voit myös määrittää **PSCredential** objektin niin, että käyttäjän toimia ei tarvita. Sinun on ensin suojattua salasanan määrittäminen. Voit aloittaa vain teksti-salasanan muunna se käyttämällä [ConvertTo SecureString](https://technet.microsoft.com/library/hh849818.aspx)suojatun merkkijonon, joka määrittää. Seuraavaksi sinun täytyy suojatun merkkijono muuntaminen salattuja vakio merkkijonon käyttäminen [ConvertFrom SecureString](https://technet.microsoft.com/library/hh849814.aspx). Nyt voit tallentaa tiedoston [Määrittäminen sisällön](https://technet.microsoft.com/library/ee176959.aspx)salattuja vakio merkkijono.

Voit myös luoda tiedoston suojattua salasanan niin, että sinun ei tarvitse kirjoittaa salasana aina. Suojattua salasanan tiedoston on myös parempi kuin tavallinen tekstitiedosto. Seuraavat PowerShellin avulla voit luoda suojattua salasanan tiedoston:

```
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

>[AZURE.IMPORTANT] Kun määrität salasanan, varmista, että [monimutkaisuusvaatimukset](https://technet.microsoft.com/library/cc786468.aspx)täyttyvät.

Voit luoda tunnistetiedon objektin suojattua salasanan tiedostosta lukea tiedoston sisältöä ja muuntaa ne suojatun merkkijonon käyttäminen [ConvertTo SecureString](https://technet.microsoft.com/library/hh849818.aspx).

[Määritä AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) cmdlet-komento hyväksyy myös *vanheneminen* -parametri, joka määrittää **päivämäärä ja aika** , jolla käyttäjätili vanhenee. Voit esimerkiksi määrittää tilin vanhenemassa muutaman päivän päivämäärän ja kellonajan.

PowerShell-esimerkin avulla voit määrittää Remote Desktop-tunniste pilvipalveluun:

```
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
Voit myös määrittää käyttöönoton paikka ja roolien, jonka haluat ottaa käyttöön etätyöpöytä. Jos nämä parametrit ei ole määritetty, cmdlet mahdollistaa Etätyöpöytä-rooleista **tuotannon** käyttöönoton paikka.

Etätyöpöytä-tunniste on liitetty käyttöönoton. Jos luot uuden käyttöönotto-palvelun, sinun on otettava etätyöpöydän kyseisen ympäristöön. Jos haluat aina Etätyöpöytä käytössä, valitse Ota huomioon PowerShell-komentosarjojen integroiminen käyttöönoton työnkulun.


## <a name="remote-desktop-into-a-role-instance"></a>Etätyöpöytä kyselyjä rooli-esiintymä
[Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) cmdlet-komento käytetään etätyöpöydän kyselyjä pilvipalvelussa sijaitsevaan rooli esiintymä. Voit käyttää *LocalPath* -parametrin Lataa paikallisesti RDP-tiedosto. Tai *Käynnistä* -parametrin avulla voit käynnistää suoraan käyttämään cloud palvelun roolin esiintymän Etätyöpöytäyhteys-valintaikkuna.

```
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```


## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a>Jos etätyöpöydän tunniste on käytössä-palveluun tarkistaminen
[Get-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495261.aspx) cmdlet-komento, joka näyttää Etätyöpöytä käytössä vai ei service-ympäristöön. Cmdlet palauttaa käyttäjänimi remote työpöydän käyttäjä-ja roolit, joilla on remote työpöydän tunniste on otettu käyttöön. Oletusarvoisesti tämä tapahtuu käyttöönoton paikka ja voit käyttää väliaikaisen paikan sijaan.

```
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a>Poista etätyöpöydän tunniste-palvelusta
Jos olet ottanut käyttöönoton remote työpöydän tunniste ja on päivitettävä remote työpöydän asetuksia, poistettava tunniste. Ja ottaa sen uudelleen käyttöön uusia asetuksia. Jos esimerkiksi haluat määrittää uuden remote käyttäjätilin salasanan tai tili vanhentunut. Tähän tarvitaan aiemmin käyttöönotoissa, joissa on käytössä remote työpöydän tunniste. Uusien versioiden voit vain käyttää tunnisteen suoraan.

Voit poistaa remote työpöydän tunniste käyttöönoton, [Poista AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495280.aspx) cmdlet-komento. Voit myös määrittää käyttöönoton paikka ja rooli, josta haluat poistaa remote työpöydän tunniste.

```
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

>[AZURE.NOTE] Poistaa tunnisteen määritystä, soita *Poista* cmdlet- **UninstallConfiguration** -parametrin kanssa.
>
>**UninstallConfiguration** -parametri poistaa tunnisteen kokoonpanoa, jota käytetään palveluun. Jokaisen tunniste-määritys on liitetty palvelun määrityksiä. Puhelut *Poista* cmdlet-komento ilman **UninstallConfiguration** kätevämpi <mark>käyttöönoton</mark> tunniste määrityksestä, näin tehokkaasti poistaminen tunniste. Tunniste-määritysten pysyy liittyvät-palvelussa.



## <a name="additional-resources"></a>Lisäresursseja

[Pilvipalveluihin määrittäminen](cloud-services-how-to-configure.md)
