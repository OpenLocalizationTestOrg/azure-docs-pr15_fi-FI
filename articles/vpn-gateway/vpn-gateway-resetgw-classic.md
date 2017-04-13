<properties
   pageTitle="Palauttaa Azure VPN-yhdyskäytävän | Microsoft Azure"
   description="Tässä artikkelissa käydään läpi Azure VPN-yhdyskäytävän palauttaminen. Artikkeli koskee VPN-yhdyskäytäviä, sekä perinteinen ja resurssien hallinnan käyttöönotto-mallit."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="cherylmc"/>

# <a name="reset-an-azure-vpn-gateway-using-powershell"></a>Palauta VPN Azure-yhdyskäytävän PowerShellin avulla


Tässä artikkelissa käydään läpi palauttamisesta käyttämäsi Azure VPN-yhdyskäytävän käyttämällä PowerShell cmdlet-komennot. Nämä ohjeet ovat perinteinen käyttöönottomalli ja resurssien hallinnan käyttöönottomalli.

Azure VPN-yhdyskäytävän palauttaminen on hyödyllinen, jos Valitse vähintään yksi S2S VPN-tunneleita paikallisen-VPN-yhteys katkeaa. Tässä tilanteessa paikallisen VPN-laitteiden kaikki toimivat oikein, mutta ei pysty muodostamaan IP tunneleita Azure VPN-yhdyskäytävät kanssa. 

Kunkin Azure VPN-yhdyskäytävän koostuu kaksi AM esiintymää on aktiivinen valmius-määritys. PowerShell-cmdlet-komennon avulla voit palauttaa yhdyskäytävän, kun se käynnistyy yhdyskäytävän ja käyttää sitä paikallisen--määrityksiä. Yhdyskäytävän pitää se on jo julkinen IP-osoite. Tällöin sinun ei tarvitse päivittää VPN-yhteyttä reitittimen määritys uuteen julkiseen IP-osoite Azure VPN Gatewayn.  

Kun komento on annettu, Azure VPN-yhdyskäytävän nykyisen esiintymän aktiiviset käynnistetään heti. Hakutuloksissa näkyy lyhyt välin aikana vikasietotilaa aktiivinen esiintymästä (on käynnistettävä), valmius-esiintymässä. Väliä on oltava alle minuutti.

Jos yhteyttä ei ole palautettu ensimmäisen uudelleenkäynnistyksen jälkeen, antaa sama komento uudelleen uudelleen toisen AM esiintymän (uusi aktiivinen yhdyskäytävän). Jos kaksi käynnistyy pyydetään takaisin, taaksepäin, ole hieman pidempään missä sekä AM esiintymät (aktiivinen ja valmiustilassa) ovat on käynnistettävä. Tämä aiheuttaa pidempi välin VPN-yhteys enintään 2, 4 minuuttia VMs suorittamiseen käynnistyy.

Kaksi käynnistyy, kun Jos kohtaat edelleen paikallisen-yhteysongelmien, Avaa tukipyynnön Azure-portaalista.

## <a name="before-you-begin"></a>Ennen aloittamista

Ennen kuin voit palauttaa käyttämäsi yhdyskäytävän, tarkista tärkeimmät kohdat, kunkin IP-,-sivusto (S2S) VPN-tunnelin. Kaikki kohteet ristiriita johtaa S2S VPN tunneleita Katkaise. Tarkistaminen ja korjaaminen paikallisen ja Azure VPN yhdyskäytävien käyttömahdollisuudet tallentaa tarpeettomat käynnistyy ja muut yhteydet toimimasta yhdyskäytävät-keskeytyksiä.

Varmista ennen käyttämäsi yhdyskäytävän palauttamista seuraavia kohteita:

- Internet-IP-osoitteet (VIPs) Azure VPN-yhdyskäytävän ja paikallisen VPN-yhdyskäytävän on määritetty oikein Azure ja paikallisen VPN-käytännöt.
- Ennalta jaetun avaimen on oltava sama Azuren ja paikallisten VPN-yhdyskäytävää.
- Jos haluat käyttää tiettyä IP/IKE määritys-salausta, hajautuksessa algoritmien ja PFS (sopivaa eteenpäin salaisuus), kuten varmistaa Azuren ja paikallisten VPN-yhdyskäytävät on sama määrityksiä.

## <a name="reset-a-vpn-gateway-using-the-resource-management-deployment-model"></a>Resurssienhallinta käyttöönoton mallin VPN-yhdyskäytävän palauttaminen

Yhdyskäytävän palauttamiseksi Resurssienhallinta PowerShell-cmdlet-komento on `Reset-AzureRmVirtualNetworkGateway`. Seuraava esimerkki palauttaa Azure VPN yhdyskäytävän, "VNet1GW" resurssiryhmä "TestRG1".

    $gw = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroup TestRG1
    Reset-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw

## <a name="reset-a-vpn-gateway-using-the-classic-deployment-model"></a>Palauta VPN yhdyskäytävän perinteinen käyttöönottomalli

Palauttamiseksi Azure VPN-yhdyskäytävän PowerShell-cmdlet-komento on `Reset-AzureVNetGateway`. Seuraava esimerkki palauttaa Azure-VPN-yhdyskäytävän nimeltä "ContosoVNet" virtual verkossa.
 
    Reset-AzureVNetGateway –VnetName “ContosoVNet” 

Tulos:

    Error          :
    HttpStatusCode : OK
    Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
    Status         : Successful
    RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
    StatusCode     : OK


## <a name="next-steps"></a>Seuraavat vaiheet
    
Katso [PowerShell hallinnan cmdlet-viittaus](https://msdn.microsoft.com/library/azure/mt617104.aspx) ja lisätietoja [Resurssienhallinta PowerShell-cmdlet-viittaus](http://go.microsoft.com/fwlink/?LinkId=828732) .






