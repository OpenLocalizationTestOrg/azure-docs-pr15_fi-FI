<properties 
    pageTitle="Miten voit ohjata USB-laitteiden Azure RemoteApp? | Microsoft Azure" 
    description="Opettele käyttämään uudelleenohjaus Azure RemoteApp USB-laitteissa." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-do-you-redirect-usb-devices-in-azure-remoteapp"></a>Miten voit ohjata USB-laitteiden Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Laitteen uudelleenohjaus avulla käyttäjät voivat käyttää USB-laitteet, joka on liitetty tietokoneella tai -tabletti, jonka Azure RemoteApp sovellukset. Esimerkiksi jos jaetut – Azure RemoteApp Skype-käyttäjien on voi käyttää heidän laitteen kamerat.

Ennen kuin jatkat, varmista, että luet [Azure RemoteApp uudelleenohjauksen käyttäminen](remoteapp-redirection.md)USB uudelleenohjauksen tiedot. Kuitenkin on suositeltavaa nusbdevicestoredirect:s: * eivät sovellu USB-web-kamerat ja eivät välttämättä toimi joillekin USB-tulostimien tai USB-älykkäät laitteet. Rakenne ja tietoturvasyistä Azure RemoteApp-järjestelmänvalvojan pitää käyttöön uudelleenohjaus laitteen luokan GUID-tunnus tai laitteen Esiintymätunnus, ennen kuin käyttäjät voivat käyttää kyseiset laitteet.

Tässä artikkelissa on lyhyt web kameran uudelleenohjaus, vaikka voit käyttää samaa menetelmää uudelleenohjaaminen USB-tulostimien ja muissa USB älykkäät, joka ei ole ohjannut sen eteenpäin **nusbdevicestoredirect:s:*** komento.

## <a name="redirection-options-for-usb-devices"></a>USB-laitteiden uudelleenohjauksen asetukset
Azure RemoteApp käyttää hyvin samankaltaisia kuin järjestelmiä uudelleenohjausta USB-laitteet muodossa käytettävissä olevia Etätyöpöytäpalvelut varten. Pohjana oleva tekniikka voit valita oikean uudelleenohjaus menetelmä laitteen, saat parhaan molemmat korkean tason ja RemoteFX USB-laitteen uudelleenohjaus avulla **usbdevicestoredirect:s:** komento. On neljä osaa tämän komennon:

| Käsittelyjärjestyksen | Parametri           | Kuvaus                                                                                                                |
|------------------|---------------------|----------------------------------------------------------------------------------------------------------------------------|
| 1                | *                   | Valitsee kaikki laitteet, jotka eivät ole poimia korkean tason uudelleenohjaus mukaan. Huomautus: suunniteltu ominaisuus * ei sovi web USB-kamerat.  |
|                  | {Laitteen luokan GUID} | Valitsee kaikki laitteet, jotka vastaavat määritettyä laitetta asetukset-luokan.                                                           |
|                  | USB\InstanceID      | Valitsee USB-laitteen määritetty annetun esiintymän ID-tunnuksellasi.                                                                  |
| 2                | -USB\Instance tunnus    | Poistaa määritetyn laitteen uudelleenohjauksen asetuksia.                                                                 |

## <a name="redirecting-a-usb-device-by-using-the-device-class-guid"></a>USB-laitteen uudelleenohjausta käyttämällä laitteen luokan GUID-tunnus
Etsi laitteen luokan GUID-tunnus, joita voidaan käyttää uudelleenohjauksen kahdella eri tavalla. 

Ensimmäinen vaihtoehto on käyttää [System-Defined laitteen asennuksen luokkien toimittajien käytettävissä](https://msdn.microsoft.com/library/windows/hardware/ff553426.aspx). Valitse luokka, joka vastaa parhaiten paikalliseen tietokoneeseen laitteita. Digitaaliset kamerat kohdalla voi olla Imaging laitteen luokka- tai videolaite siepata luokka. Sinun on suoritettava joitakin vuorovaikutteisuudesta laitteen luokkia voit etsiä oikea luokan GUID-tunnus, joka toimii paikallisesti liitetty USB-laite (Kirjoita tässä tapauksessa web-kamera) kanssa.

Paremman tavan tai toinen vaihtoehto on voit etsiä tietyn laitteen luokan GUID-tunnus seuraavasti:

1. Avaa Laitehallinta, Etsi laitteella, joka ohjataan ja napsauta sitä hiiren kakkospainikkeella ja valitse Ominaisuudet.
![Avaa Laitehallinta](./media/remoteapp-usbredir/ra-devicemanager.png)
2. Valitse **tiedot** -välilehden **Luokan GUID-tunnus**-ominaisuutta. Arvo, joka näkyy on kyseisen laitteen tyyppiä luokan GUID-tunnus.
![Kameran ominaisuudet](./media/remoteapp-usbredir/ra-classguid.png)
3. Uudelleenohjata laitteita, jotka vastaavat sen luokan Guid-arvoksi.

Esimerkki:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s:<Class Guid value>"

Voit yhdistää useita saman cmdlet-laitteen uudelleenohjausta. Esimerkki: paikallisen säilytys- ja USB-web-kamera ohjaaminen cmdlet-komento näyttää tältä:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:<Class Guid value>"

Kun määrität laitteen uudelleenohjaus luokan GUID ohjataan kaikissa laitteissa, jotka vastaavat määritettyä sivustokokoelman kyseisen luokan GUID-tunnus. Jos määritettynä on lähiverkossa useissa tietokoneissa, joissa on sama USB-web-kamerat, voit suorittaa yhteen cmdlet-komento, voit ohjata kaikki web-kamerat.

## <a name="redirecting-a-usb-device-by-using-the-device-instance-id"></a>USB-laitteen uudelleenohjausta käyttämällä laitteen tunniste

Jos haluat monipuolisempaa ja haluat määrittää uudelleenohjauksen laite, voit käyttää **USB\InstanceID** uudelleenohjaus parametrin.

Tämä menetelmä vaikein osa on etsiminen USB laitteen esiintymän ID-tunnuksellasi. Sinun on pääsy tietokoneen ja USB-laitetta. Toimi sitten seuraavasti:

1. Ota työpöydän etäistunnon laitteen-uudelleenohjaus kuvatulla tavalla [siitä, miten käytetään Omat laitteet ja resurssit etätyöpöydän istunnon?](http://windows.microsoft.com/en-us/windows7/How-can-I-use-my-devices-and-resources-in-a-Remote-Desktop-session)
2. Avaa Etätyöpöytäyhteys ja valitse **Näytä asetukset**.
3. Valitse **Tallenna nimellä** tallentaa nykyiset yhteysasetukset RDP-tiedostoon.  
    ![Tallenna asetukset RDP-tiedostona](./media/remoteapp-usbredir/ra-saveasrdp.png)
4. Valitse tiedostonimen ja sijainnin, esimerkiksi "MyConnection.rdp" ja "Tämä PC\Documents", ja Tallenna tiedosto.
5. Avaa tekstieditorissa MyConnection.rdp-tiedosto ja Etsi tunniste ohjattavaa laitteen.

Nyt käyttää tunniste seuraavan cmdlet-komennon:

    Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s: USB\<Device InstanceID value>"



### <a name="help-us-help-you"></a>Auta meitä auttamaan 
Tiesitkö, että lisäksi tässä artikkelissa arviointi ja tehdä kommentit alaspäin alla, voit tehdä muutoksia itse artikkelin? Järjestelmässä ei löydy? Ongelmia? Kirjoittaa jotakin, mitä on vain hankalaa? Vierittää ylöspäin ja valitse muokkaamiseen **GitHub muokkaaminen** - ne toimitetaan US tarkastelua varten ja sitten, kun olemme kirjautua niitä, näet muutokset ja parannukset täältä.