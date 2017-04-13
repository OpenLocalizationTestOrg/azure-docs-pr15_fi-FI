<properties
   pageTitle="Luoda mukautetun näytteenottimen sovelluksen Gatewayn portaalissa | Microsoft Azure"
   description="Opi voit luoda mukautetun näytteenottimen sovelluksen Gateway-portaalissa"
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

# <a name="create-a-custom-probe-for-application-gateway-by-using-the-portal"></a>Mukautetun näytteenottimen luominen sovelluksen Gateway-portaalissa

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-create-probe-portal.md)
- [Azure Resurssienhallinta PowerShell](application-gateway-create-probe-ps.md)
- [Azure perinteinen PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

## <a name="scenario"></a>Skenaario

Seuraava tilanne käy läpi mukautetun kunto näytteenottimen luominen olemassa olevan sovelluksen-yhdyskäytävä.
Skenaario oletetaan, että olet jo tehnyt ohjeita voit [luoda yhdyskäytävän sovelluksen](application-gateway-create-gateway-portal.md).

## <a name="createprobe"></a>Luo näytteenottimen

Näytteenottimet on määritetty kaksivaiheinen prosessi portaalin kautta. Ensimmäiseksi on luotava näytteenottimen, seuraavaksi näytteenottimen lisääminen sovelluksen yhdyskäytävän Taustajärjestelmä http-asetuksia.

### <a name="step-1"></a>Vaihe 1

Siirry http://portal.azure.com ja valitse aiemmin luotu sovelluksen-yhdyskäytävä.

![Sovelluksen Gateway yleiskatsaus][1]

### <a name="step-2"></a>Vaihe 2

Valitse **Probes** ja valitse Lisää uusi näytteenottimen **Lisää** -painiketta.

![Lisää näytteenottimen sivu, joka sisältää tiedot on täytetty][2]

### <a name="step-3"></a>Vaihe 3

Täytä näytteenottimen tarvittavat tiedot, ja kun olet valmis, valitse **OK**.

- **Nimi** - tämä on kutsumanimi, jota voi käyttää portaalissa näytteenottimen.
- **Host (isäntä)** - tämä on isäntänimi, jota käytetään näytteenottimen. Käytettävissä vain, kun usean sivuston on määritetty sovelluksen yhdyskäytävässä, muuten käyttää "127.0.0.1". Tämä on sama kuin AM isäntänimi.
- **Path** - loput mukautetun keräysputken koko URL-osoite. Virheellinen polun alussa on "/"
- **Väli (s)** - kuinka usein näytteenottimen suoritetaan kunto tarkistavan. Ei ole suositeltavaa Määritä pienempi kuin 30 sekuntia.
- **Aikakatkaisu (s)** - ajan näytteenottimen odottaa ennen aikakatkaisua. Aikavälin on oltava riittävän http-kutsu voidaan varmistaa Taustajärjestelmä kunto-sivulla on käytettävissä.
- **Perusasemassa raja** - katsotaan perusasemassa epäonnistuneiden yritysten määrä. Raja-arvo on 0 tarkoittaa, että, jos kuntotietojen tarkistuksessa epäonnistuu sitten taustatietokantatiedosto määritetään perusasemassa heti.

> [AZURE.IMPORTANT] isäntänimi ei ole palvelimen nimi. Tämä on sovelluksen palvelimessa virtual isännän nimen. Näytteenottimen lähetetään http://(host name):(port from httpsetting)/urlPath

![näytteenottimen asetukset][3]

## <a name="add-probe-to-the-gateway"></a>Lisää näytteenottimen Gateway

Nyt kun näytteenottimen on luotu, on aika lisääminen yhdyskäytävän. Näytteenottimen asetukset on määritetty sovelluksen yhdyskäytävän Taustajärjestelmä http-asetukset.

### <a name="step-1"></a>Vaihe 1

Valitse sovelluksen yhdyskäytävän **HTTP-asetukset** ja valitse sitten nykyisen taustatietokannan http asetukset-ikkunassa ja tuo näkyviin määritys-sivu.

![HTTPS-asetukset-ikkunassa][4]

### <a name="step-2"></a>Vaihe 2

**AppGatewayBackEndHttp** asetukset-sivu, valitse **Käytä mukautettuja näytteenottimen** ja valitse keräysputken luotu [Luo näytteenottimen](#createprobe) -osassa.
Kun olet valmis, valitse **OK** ja asetukset otetaan käyttöön.

![appgatewaybackend asetukset-sivu][5]

Oletus-keräysputken tarkistaa oletuskäyttöoikeudet web-sovelluksen. Nyt kun mukautettu näytteenottimen on luotu, sovelluksen yhdyskäytävä käyttää valittuna Taustajärjestelmä kunnon valvonta määritetty liikerata. Sovelluksen yhdyskäytävän tarkistaa näytteenottimen määritetyn tiedoston, joka on määritetty ehtojen perusteella. Jos isäntä: portti puhelun / polun ei palauta Http 200 OK-tila-vastauksen-palvelin otetaan kierron, kun perusasemassa raja-arvo on saavutettu. Tutkitaan säilyy perusasemassa esiintymässä määrittääksesi, kun se on kunnossa uudelleen. Kun esiintymää on lisätty takaisin kunnossa palvelimen resurssivarantoon liikenne alkaa juoksutus siihen uudelleen ja tutkitaan esiintymään sijoittuu käyttäjän määritetyn aikavälin normaalisti.


## <a name="next-steps"></a>Seuraavat vaiheet

Opettele määrittämään SSL purkaminen Azure Application Gatewayn artikkelissa [Määrittäminen SSL purku](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-probe-portal/figure1.png
[2]: ./media/application-gateway-create-probe-portal/figure2.png
[3]: ./media/application-gateway-create-probe-portal/figure3.png
[4]: ./media/application-gateway-create-probe-portal/figure4.png
[5]: ./media/application-gateway-create-probe-portal/figure5.png