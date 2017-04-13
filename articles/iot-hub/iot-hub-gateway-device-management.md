<properties
 pageTitle="Hallitut laitteet IoT yhdyskäytävän takana | Microsoft Azure"
 description="Ohjeaiheen IoT yhdyskäytävän avulla luotu IoT yhdyskäytävän SDK sekä hallitsee IoT keskittimeen laitteet."
 services="iot-hub"
 documentationCenter=""
 authors="chipalost"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="cstreet"/>
 
# <a name="enable-managed-devices-behind-an-iot-gateway"></a>Hallittujen laitteiden takana IoT yhdyskäytävän ottaminen käyttöön

## <a name="basic-device-isolation"></a>Laitteen eristystaso

Organisaatiot käyttävät usein IoT yhdyskäytävien niin, että niiden IoT ratkaisuja tietoturvan. Joitakin laitteita lähetettävä tiedot pilvipalveluun, mutta eivät voi suojaaminen itsestään uhkien Internetissä. Voit suojata seuraaviin laitteisiin ulkoisen viestiketjuissa siirtyminen-niitä yhteydenpito ulkopuolelta maailman yhdyskäytävän kautta.

Yhdyskäytävä on avattujen suojatun ympäristön ja avaa internet välisen reunan. Yhdyskäytävän jutella laitteet ja yhdyskäytävän välittää viestit pitkin oikea pilven kohteeseen. Yhdyskäytävä on vahvistettu vastaan ulkoisen viestiketjuissa siirtyminen, estää luvattomia pyynnöt, sallii valtuutettujen-sidottujen liikenteen ja välittää sidottu-liikenne oikeaan laitteeseen.

![][1]

Voit myös siirtää laitteita, jotka voit suojata itse yhdyskäytävä lisättyä kerroksen arvopaperin takana. Tässä skenaariossa vain haluat säilyttää yhdyskäytävän OS asennettu uusimmat heikkouksien sijaan päivitetään jokaisen laitteessa OS vastaan.

## <a name="isolation-plus-intelligence"></a>Eristystaso plus liiketoimintatietojen

Vahvistettu reitittimen on riittävä yhdyskäytävän eristää riittää, että laitteet. Kuitenkin IoT ratkaisuja usein edellyttää, että yhdyskäytävän on enemmän kuin eristämisestä riittää, että laitteet liiketoimintatietojen. Jos esimerkiksi haluat Hallitse pilvestä. Olet voi käyttää [LWM2M](https://github.com/OpenMobileAlliance/OMA_LwM2M_for_Developers/wiki), vakio laitteen management-protokollaa ratkaisun cloud hallinta-osassa. Kuitenkin laitteet lähettää ei TCP/IP käyttämällä telemetriatietojen käytössä protokolla. Lisäksi laitteet tuottaa paljon tietoja ja haluat ladata telemetriatietojen suodatetut alijoukkoa. Voit luoda ratkaisun, joka kattaa IoT yhdyskäytävän voi käsittely kaksi eri virtaa tietojen kanssa. Yhdyskäytävä on:

-   **Telemetriatietojen**ymmärtää, suodattamista ja lataa se sitten pilveen yhdyskäytävän kautta. Yhdyskäytävä ei ole enää yksinkertainen reitittimen, joka välittää riittää, että laitteen ja pilveen tietojen.

-   Vain exchange laitteet ja pilveen **LWM2M laitteen tietojen** . Yhdyskäytävä ei tarvitse ymmärtää siihen tulevat tiedot ja varmista, että tiedot on siirretty edestakaisin laitteet ja pilveen välillä on vain.

Tässä skenaariossa seuraavassa kuvassa:

![][2]

## <a name="the-solution-azure-iot-device-management-and-the-iot-gateway-sdk"></a>Ratkaisu: Azure IoT hallinta ja IoT yhdyskäytävän SDK-paketissa 

Yleisön esikatsella julkaisua [Azure IoT hallinta] [ lnk-device-management] ja [Azure IoT yhdyskäytävän SDK] beetaversio Ota tämä skenaario. Yhdyskäytävän käsittelee kunkin stream tietoja seuraavasti:

-   **Telemetriatietojen**: IoT yhdyskäytävän SDK avulla voit luoda myyntijakso, ymmärtää, suodattaa ja lähettää telemetriatietoja pilveen. IoT yhdyskäytävän SDK on koodi, joka sisältää tässä myyntijakso puolesta kehittäjä osat. Löydät lisätietoja SDK arkkitehtuuri [IoT yhdyskäytävän SDK - Aloita] [ lnk-gateway-get-started] opetusohjelma.

-   **Hallinta**: Azure hallinta sisältää LWM2M-asiakas, joka suoritetaan laitteen sekä cloud-liittymän myöntämistä hallinta komennot laitteeseen.
    
    Ei edellytä mitään erityistä logiikan yhdyskäytävän, koska se ei tarvitse käsitellä välillä laitteen ja IoT-toiminnossa LWM2M tietoja. Voit ottaa internet-yhteyden jakamisesta on monta Moderni-käyttöjärjestelmissä yhdyskäytävän vaihtamiseen LWM2M tietojen ominaisuus. Voit valita sopivan käyttöjärjestelmän tämän skenaarion, koska IoT yhdyskäytävän SDK tukee monenlaisia käyttöjärjestelmät. Seuraavassa on ohjeet internet-yhteyden [Windows 10: ssä] ja [Ubuntu], kaksi monta tuetut käyttöjärjestelmät jakamisen ottaminen käyttöön.

Seuraavassa kuvassa käytetään artikkelista [Azure IoT hallinnan] käyttäminen korkean tason arkkitehtuuri[ lnk-device-management] ja [Azure IoT yhdyskäytävän SDK].

![][3]

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja IoT yhdyskäytävän SDK käyttämisestä on artikkelissa opetusohjelmassa:

- [Yhdyskäytävän IoT SDK - Linux käytön aloittaminen][lnk-gateway-get-started]
- [Yhdyskäytävän IoT SDK – lähettämiseen laitteen pilven avulla Linux Simuloitu laitteen kanssa][lnk-gateway-simulated]

Tutustu tarkemmin IoT keskittimeen ominaisuuksia, katso:

- [Sovelluskehittäjän opas][lnk-devguide]
- [Simuloimalla laitetta, jonka IoT yhdyskäytävän SDK-paketissa][lnk-gateway-simulated]

<!-- Images and links -->
[1]: media/iot-hub-gateway-device-management/overview.png
[2]: media/iot-hub-gateway-device-management/manage.png
[Azure IoT yhdyskäytävän SDK-paketissa]: https://github.com/Azure/azure-iot-gateway-sdk/
[Windows 10: ssä]: http://windows.microsoft.com/en-us/windows/using-internet-connection-sharing#1TC=windows-7
[Ubuntu]: https://help.ubuntu.com/community/Internet/ConnectionSharing
[3]: media/iot-hub-gateway-device-management/manage_2.png
[lnk-gateway-get-started]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-gateway-simulated]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-device-management]: iot-hub-device-management-overview.md

[lnk-devguide]: iot-hub-devguide.md