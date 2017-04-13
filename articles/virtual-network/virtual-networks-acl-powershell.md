<properties
   pageTitle="Hallinta Access ohjausobjektin luettelot (käyttöoikeusluettelot) päätepisteiden PowerShell-toiminnolla"
   description="Opi hallitsemaan käyttöoikeusluettelot PowerShellin avulla"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-manage-access-control-lists-acls-for-endpoints-by-using-powershell"></a>Hallinta Access ohjausobjektin luettelot (käyttöoikeusluettelot) päätepisteiden PowerShell-toiminnolla

Voit luoda ja hallita verkon käytön hallinta luettelot (käyttöoikeusluettelot) päätepisteiden Azure PowerShell-toiminnolla tai hallinta-portaalissa. Tässä ohjeaiheessa Käyttöoikeusluettelon usein käytettyjen tehtävien, jotka voivat tehdä PowerShellin avulla löydät ohjeita. Katso Azure PowerShell cmdlet-komentojen luettelo [Azure hallinnan cmdlet-komennot](http://go.microsoft.com/fwlink/?LinkId=317721). Saat lisätietoja käyttöoikeusluettelot [verkon luettelon Käyttöoikeusluettelon (Access Control) ominaisuudet?](virtual-networks-acl.md). Jos haluat hallita oman käyttöoikeusluettelot hallinta-portaalissa, katso, [miten voit määrittää määrittäminen päätepisteet Virtual koneeseen](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

## <a name="manage-network-acls-by-using-azure-powershell"></a>Hallitse verkon käyttöoikeusluettelot Azure PowerShell-toiminnolla

Voit käyttää Azure PowerShellin cmdlet-komennot luominen, poistaminen ja määritä (Aseta) verkon käytön hallinta luettelot (käyttöoikeusluettelot). Olemme olet kirjoittanut muutamia esimerkkejä joitakin tapoja, joilla voit määrittää Käyttöoikeusluettelon PowerShellin avulla.

Hakemiseen luettelo kaikista Käyttöoikeusluettelon PowerShellin cmdlet-komennot jommallakummalla seuraavista tavoista:

    Get-Help *AzureACL*
    Get-Command -Noun AzureACLConfig

### <a name="create-a-network-acl-with-rules-that-permit-access-from-a-remote-subnet"></a>Luo säännöt, jotka sallivat access remote aliverkosta verkon Käyttöoikeusluettelon

Seuraavassa esimerkissä on kuvattu voi luoda uuden Käyttöoikeusluettelon, joka sisältää sääntöjä. Tämän Käyttöoikeusluettelon käytetään sitten virtuaalikoneen päätepiste. Alla olevassa esimerkissä Käyttöoikeusluettelon sääntöjen avulla voi käyttää remote aliverkosta. Jos haluat luoda uuden verkon Käyttöoikeusluettelon sallia sääntöjen remote aliverkon, Avaa Azure PowerShell ise-:. Kopioi ja liitä komentosarja alla komentosarja määrittäminen oman arvoilla ja suorittamalla komentosarja.

1. Luo uusi verkon Käyttöoikeusluettelon objekti.

        $acl1 = New-AzureAclConfig

1. Määritä säännön, joka sallii access remote aliverkosta. Alla olevassa esimerkissä voit määrittää säännön *100* (joka on yli 200 tai uudempi versio säännön prioriteetti) remote aliverkon *10.0.0.0/8* annettavien virtuaalikoneen päätepiste. Korvaa arvot määritys-tarpeen. Nimi "SharePoint Käyttöoikeusluettelon config" korvataan kutsumanimi, johon haluat soittaa tätä sääntöä.

        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"

1. Toista lisäsääntöjä-cmdlet-arvojen korvaaminen määritys-tarpeen. Varmista, että säännön muuttaminen vastaamaan järjestyksessä, jossa haluat sääntöjä käytetään tilauksen numero. Säännön pienempi luku on suurempi luku nähden.

        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"

1. Seuraavaksi voit luoda uuden päätepisteen (Lisää) tai määrittää Käyttöoikeusluettelon aiemmin päätepisteen (määritetty). Tässä esimerkissä on uusi virtuaalikoneen päätepiste nimeltä "WWW" lisätään ja päivitetään virtuaalikoneen päätepisteen Käyttöoikeusluettelon asetusten kanssa.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        | Update-AzureVM

1. Seuraavaksi yhdistää Cmdlet-komentoja ja Suorita komentosarja. Tässä esimerkissä yhdistetyn cmdlet-komennot näyttää tältä:

        $acl1 = New-AzureAclConfig
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "Sharepoint ACL config"
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        |Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        |Update-AzureVM

### <a name="remove-a-network-acl-rule-that-permits-access-from-a-remote-subnet"></a>Verkon Käyttöoikeusluettelon säännön, joka sallii access remote aliverkosta poistaminen

Seuraavassa esimerkissä on kuvattu tapa poistaa verkon Käyttöoikeusluettelon säännön.  Jos haluat poistaa remote aliverkon säännöt Salli verkon Käyttöoikeusluettelon sääntö, Avaa Azure PowerShell ise-:. Kopioi ja liitä komentosarja alla komentosarja määrittäminen oman arvoilla ja suorittamalla komentosarja.

1. Ensimmäiseksi on verkon Käyttöoikeusluettelon objektin hakeminen virtuaalikoneen päätepiste. Valitse Poista Käyttöoikeusluettelon sääntö. Tässä tapauksessa emme ovat poistamalla se säännön ID-tunnuksellasi. Tämä poistaa vain Sääntötunnus 0 Käyttöoikeusluettelon. Se ei poista virtuaalikoneen päätepisteen Käyttöoikeusluettelon objekti.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Get-AzureAclConfig –EndpointName "web" `
        | Set-AzureAclConfig –RemoveRule –ID 0 –ACL $acl1

1. Seuraavaksi on käyttää verkon Käyttöoikeusluettelon objektin virtuaalikoneen päätepisteen ja Päivitä virtuaalikoneen.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Set-AzureEndpoint –ACL $acl1 –Name "web" `
        | Update-AzureVM

### <a name="remove-a-network-acl-from-a-virtual-machine-endpoint"></a>Poista virtuaalikoneen päätepisteen verkon Käyttöoikeusluettelon

Tietyissä tilanteissa haluat ehkä verkon Käyttöoikeusluettelon objektin poistaminen virtuaalikoneen päätepiste. Saadakseen ne Avaa Azure PowerShell ise-:. Kopioi ja liitä komentosarja alla komentosarja määrittäminen oman arvoilla ja suorittamalla komentosarja.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Remove-AzureAclConfig –EndpointName "web" `
        | Update-AzureVM

## <a name="next-steps"></a>Seuraavat vaiheet

[Mikä verkon luettelon Käyttöoikeusluettelon (Access Control)?](virtual-networks-acl.md)
