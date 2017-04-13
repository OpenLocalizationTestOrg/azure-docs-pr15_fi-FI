<properties
   pageTitle="Sovelluksen yhdyskäytävän polku perustuvan säännön luominen portaalissa | Microsoft Azure"
   description="Opettele portaalissa sovelluksen yhdyskäytävän polku perustuvan säännön luominen"
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

# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-the-portal"></a>Portaalissa sovelluksen yhdyskäytävän polku perustuvan säännön luominen

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-create-url-route-portal.md)
- [Azure Resurssienhallinta PowerShell](application-gateway-create-url-route-arm-ps.md)

URL-Osoitteen polku-pohjainen reititys avulla voit yhdistää tiet Http-pyyntö URL-polku perusteella. Se tarkistaa, onko määritetty sovelluksen yhdyskäytävän URL-luetteloiden taustatietokantaan resurssivarantoon reitin ja Lähetä verkkoliikenteen määritetyn taustatietokantaan resurssivarantoon. Yleisen käytön URL-pohjaiset reitittämiseen on eri sisältötyyppien toisen taustatietokannan palvelimen jakavat haluat ladata.

Uuden säännön tyyppi URL-pohjaiset reititys esitellään sovelluksen yhdyskäytävä. Sovelluksen yhdyskäytävä on kaksi säännön: perus- ja polku perustuvia sääntöjä. Perustietoja sääntötyyppi aikana lisäksi PYÖRISTÄ-funktiota Mikko jakauman sääntöjen polku taustatietokantaan-jakavat pyöreän pöydän-palvelu sisältää, on myös polku malli pyyntö URL-osoitteen huomioon aikana valitsemalla Taustajärjestelmä resurssivarantoon.

## <a name="scenario"></a>Skenaario

Seuraava tilanne käy läpi polku perustuvan säännön luominen olemassa olevan sovelluksen-yhdyskäytävä.
Skenaario oletetaan, että olet jo tehnyt ohjeita voit [luoda yhdyskäytävän sovelluksen](application-gateway-create-gateway-portal.md).

![URL-osoitteen reitin][scenario]

## <a name="createrule"></a>Polun perustuvan säännön luominen

Polun perustuva sääntö edellyttää omassa listener, ennen kuin luot säännön voi varmistaa, että sinulla on käytettävissä Listenerin, jotta voit käyttää.

### <a name="step-1"></a>Vaihe 1

Siirry http://portal.azure.com ja valitse aiemmin luotu sovelluksen-yhdyskäytävä. Valitse **säännöt**

![Sovelluksen yhdyskäytävän yleiskatsaus][1]

### <a name="step-2"></a>Vaihe 2

Napsauta Lisää uusi polku perustuva sääntö **polku-pohjainen** -painiketta.

### <a name="step-3"></a>Vaihe 3

**Lisää polku perustuva sääntö** -sivu on kaksi osaa. Ensimmäinen osa on, jossa määritetty kuuntelua, polku oletusasetukset sekä säännön nimi. Polun oletusasetuksia koskevat tiet, jotka eivät kuulu mukautettu polku-pohjainen reitin. **Lisää polku perustuva sääntö** -sivu, toinen osa on, jossa voit määrittää polun perustuvia sääntöjä itse.

**Perusasetukset**

- **Nimi** - tämä on kutsumanimi, jota voi käyttää portaalissa säännön.
- **Listener** – tämä on kuuntelua, jotta sääntöä käytetään.
- **Oletusarvon Taustajärjestelmä resurssivarantoon** - asetus on asetus, joka määrittää, jota käytetään oletusarvon säännön taustatietokantaan
- **Oletusarvoinen HTTP-asetuksia** - asetus on asetus, joka määrittää HTTP-asetuksia käytetään oletusarvon säännön.

**Polun sääntöjen**

- **Nimi** - tämä on polku perustuvan säännön nimi.
- **Polut** - asetus määrittää säännön etsii liikenne välitettäessä polku
- **Taustajärjestelmä resurssivarantoon** - asetus on asetus, joka määrittää taustatietokantaan sääntöä käytetään
- **HTTP-asetus** - asetus on asetus, joka määrittää säännön käytettävä HTTP-asetuksia.

>[AZURE.IMPORTANT] Polut: Polku kuviot vastaamaan luettelo. Kunkin alettava / ja ainoa paikka "\*" sallitaan on lopussa. Kelvollinen esimerkit /xyz /xyz* tai /xyz/*.  

![Lisää polku perustuva sääntö sivu täyttää tiedot][2]

Polun perustuvan säännön lisääminen olemassa olevan sovelluksen-yhdyskäytävä on myös helppo portaalin kautta. Kun polku perustuva sääntö on luotu, se voi muokata Lisää lisäsääntöjä helposti. 

![path-pohjainen lisäsääntöjä lisääminen][3]

## <a name="next-steps"></a>Seuraavat vaiheet

Opettele määrittämään SSL purkaminen Azure Application Gatewayn artikkelissa [Määrittäminen SSL purku](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png