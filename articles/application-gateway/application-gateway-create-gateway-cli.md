<properties
   pageTitle="Luo sovelluksen yhdyskäytävän Azure-CLI resurssien hallinnan avulla | Microsoft Azure"
   description="Opettele luomaan sovelluksen yhdyskäytävän Azure-CLI resurssien hallinnan avulla"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-an-application-gateway-by-using-the-azure-cli"></a>Sovelluksen yhdyskäytävän luominen Azure-CLI avulla

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-create-gateway-portal.md)
- [Azure Resurssienhallinta PowerShell](application-gateway-create-gateway-arm.md)
- [Azure perinteinen PowerShell](application-gateway-create-gateway.md)
- [Azure Resurssienhallinta-malli](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure sovelluksen yhdyskäytävä on kerroksen 7 kuormituksen. Se sisältää automaattisesti, suorituskyvyn reititys-pyyntöjen eri palvelinten välillä, olivatpa ne sitten cloud tai paikallisen. Sovelluksen yhdyskäytävä on sovelluksen toimitus-ominaisuudet: HTTP-ladata tasaamisen, eväste perustuva istunnon affiniteetti ja Secure Sockets Layer (SSL) purku mukautetun kunto keräysputkien ja usean sivuston tuki.

## <a name="prerequisite-install-the-azure-cli"></a>Edellytyksenä: Asenna Azure CLI

Tämän artikkelin toimien, sinun on [asennettava Mac, Linux-ja (Azure CLI) Windows Azure komentorivivalitsimet-liittymän](../xplat-cli-install.md) eikä sinun tarvitse [Azure kirjautua](../xplat-cli-connect.md). 

> [AZURE.NOTE] Jos sinulla ei ole Azure-tili, sinun on yksi. Siirtyy ylöspäin Kirjaudu [Tässä maksuttoman kokeiluversion käyttäjäksi](../active-directory/sign-up-organization.md).

## <a name="scenario"></a>Skenaario

Tässä skenaariossa voit Opi luomaan sovelluksen yhdyskäytävän Azure-portaalissa.

Tässä skenaariossa on:

- Luo kaksi esiintymää Normaali sovelluksen yhdyskäytävä.
- Luo virtuaalisia verkko nimeltä AdatumAppGatewayVNET varattu CIDR tekstialueen 10.0.0.0/16 kanssa.
- Luo nimi, joka käyttää 10.0.0.0/28 sen CIDR palkkina Appgatewaysubnet aliverkon.
- Määritä SSL purku varmennetta.

![Skenaario-Esimerkki][scenario]

>[AZURE.NOTE] Sovelluksen yhdyskäytävän, mukaan lukien mukautetun kunto lisämäärityksiä probes, Taustajärjestelmä resurssivarantoon osoitteet ja lisäsääntöjä määritetään sen jälkeen, kun sovellus yhdyskäytävä on määritetty ja ei ensimmäisen käyttöönoton yhteydessä.

## <a name="before-you-begin"></a>Ennen aloittamista

Azure sovelluksen yhdyskäytävä on oltava oma aliverkon. Virtuaalinen verkoston luominen varmistaa, että jätät on useita aliverkosta tarpeeksi osoitetilaa. Kun sovellus-yhdyskäytävän aliverkon otetaan käyttöön, vain Lisää sovellus yhdyskäytävät voivat aliverkon lisättäväksi.

## <a name="log-in-to-azure"></a>Azure kirjautuminen

Avaa **Microsoft Azure komentokehote**ja kirjaudu sisään. 

    azure login

Kun kirjoitat edellisessä esimerkissä, koodi on annettu. Siirry selaimessa, voit jatkaa kirjautumisen https://aka.ms/devicelogin.

![cmd näyttäminen laitteen kirjautuminen][1]

Kirjoita saamasi koodi selaimessa. Sinut ohjataan kirjautumisen sivulle.

![koodi-selaimessa][2]

Kun koodi on syötetty olet kirjautunut sisään, sulje selain Jatka skenaariota.

![kirjautunut sisään][3]

## <a name="switch-to-resource-manager-mode"></a>Vaihda Resurssienhallinta-tilaan

    azure config mode arm

## <a name="create-the-resource-group"></a>Resurssiryhmän luominen

Ennen kuin luot sovelluksen yhdyskäytävän, resurssiryhmä luodaan sisältävät sovelluksen yhdyskäytävän. Seuraavassa kuvassa näkyy komento.

    azure group create -n AdatumAppGatewayRG -l eastus

## <a name="create-a-virtual-network"></a>Luo virtuaalisia verkko

Kun resurssiryhmän on luotu, virtual verkon luodaan sovelluksen yhdyskäytävän.  Seuraavassa esimerkissä osoitetilaa varten oli 10.0.0.0/16 kuin edellisessä skenaarion huomautukset-määritysten mukaisesti.

    azure network vnet create -n AdatumAppGatewayVNET -a 10.0.0.0/16 -g AdatumAppGatewayRG -l eastus

## <a name="create-a-subnet"></a>Luo aliverkon

Kun virtual verkko on luotu, aliverkon lisätään sovelluksen yhdyskäytävän.  Jos aiot käyttää sovelluksen yhdyskäytävän ylläpidettävä virtual samassa verkossa kuin sovelluksen yhdyskäytävän web-sovelluksen, muista jättää toisen aliverkon tilaa.

    azure network vnet subnet create -g AdatumAppGatewayRG -n Appgatewaysubnet -v AdatumAppGatewayVNET -a 10.0.0.0/28 

## <a name="create-the-application-gateway"></a>Sovelluksen yhdyskäytävän luominen

Luotuasi VPN ja aliverkon sovelluksen yhdyskäytävän vaatimat ovat valmiit. Lisäksi aiemmin viedyn .pfx-varmenne ja varmennetta salasanan varten tarvittavat seuraavat toimet: IP-osoitteet taustaan käytettäviä ovat Taustajärjestelmä palvelimen IP-osoitteet. Nämä arvot voivat olla yksityinen IP-osoitteet virtual verkossa, julkiseen IP-osoitteet tai Taustajärjestelmä palvelinten täydelliset toimialuenimet.

    azure network application-gateway create -n AdatumAppGateway -l eastus -g AdatumAppGatewayRG -e AdatumAppGatewayVNET -m Appgatewaysubnet -r 134.170.185.46,134.170.188.221,134.170.185.50 -y c:\AdatumAppGateway\adatumcert.pfx -x P@ssw0rd -z 2 -a Standard_Medium -w Basic -j 443 -f Enabled -o 80 -i http -b https -u Standard

> [AZURE.NOTE] Luettelo parametreista, joka saadaan luomista varten suorittamalla seuraavan komennon: **azure verkon sovelluksen yhdyskäytävään luominen – Ohje**.

Tässä esimerkissä luodaan basic sovelluksen yhdyskäytävän oletusasetuksilla listener, Taustajärjestelmä resurssivarantoon, Taustajärjestelmä http asetukset ja säännöt. Se myös määrittää SSL purku. Voit muuttaa näitä asetuksia haluamallasi käyttöönoton, kun valmistelun onnistuu.
Jos sinulla on jo määritetty verkkosovelluksen edellisten vaiheiden taustatietokannan, resurssivarantoon luomiasi kuormituksen tasaamisen alkaa.

## <a name="next-steps"></a>Seuraavat vaiheet

Tietokantakyselyjen luominen mukautetun kunto keräysputkien ohjesisältöä, [luoda mukautetun kunto näytteenottimen](application-gateway-create-probe-portal.md)

Lue, miten voit määrittää SSL purkaminen ja ota kallista SSL-salauksen käytöstä WWW-palvelimien ohjesisältöä [Määrittäminen SSL purku](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png