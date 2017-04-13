<properties 
    pageTitle="Tietoja live-tietovirta paikallinen koodauslaitteista Azure-portaalissa | Microsoft Azure" 
    description="Tässä opetusohjelmassa käydään läpi vaiheet, joka on määritetty läpivienti toimittamista kanavan luominen." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article"
    ms.date="10/24/2016" 
    ms.author="juliako"/>


#<a name="how-to-perform-live-streaming-with-on-premise-encoders-using-the-azure-portal"></a>Tietoja live-tietovirta paikallinen koodauslaitteista Azure-portaalissa

> [AZURE.SELECTOR]
- [Portal]( media-services-portal-live-passthrough-get-started.md)
- [.NET]( media-services-dotnet-live-encode-with-onpremises-encoders.md)
- [MUILLE KÄYTTÄJILLE]( https://msdn.microsoft.com/library/azure/dn783458.aspx)

Tässä opetusohjelmassa käydään läpi vaiheet käyttämällä Azure portaalin **kanava** , joka on määritetty läpivienti toimittamista luominen. 

##<a name="prerequisites"></a>Edellytykset

Tarvitaan suorittamiseen opetusohjelman seuraavasti:

- Azure tili. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/). 
- Media Services-tilin. Media Services-tilin luominen-kohdassa [Media Services-tilin luominen](media-services-portal-create-account.md).
- Verkkokamera. Esimerkiksi [Telestream Wirecast encoder](http://www.telestream.net/wirecast/overview.htm).

On erittäin suositeltavaa tarkistaa on seuraavissa artikkeleissa:

- [Azure Media-palveluiden RTMP tuki- ja Live koodauslaitteista](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/)
- [Yleistä Live höyryssä Azure Media-palvelujen avulla](media-services-manage-channels-overview.md)
- [Live streaming paikallinen koodauslaitteista, jotka luovat usean bittitaajuus virtaa kanssa](media-services-live-streaming-with-onprem-encoders.md)


##<a id="scenario"></a>Käytetty live streaming-vaihtoehto

Seuraavissa vaiheissa kuvataan tehtävien luomista yleisiä live streaming sovelluksia, jotka käyttävät kanavat, jotka on määritetty läpivienti toimittamista varten. Tässä opetusohjelmassa näytetään, miten voit luoda ja hallita läpivienti kanavan ja tapahtumia.

1. Kameraa muodostaa yhteyttä tietokoneeseen. Käynnistä ja määrittää paikallisen live encoder, joka siirtää usean bittitaajuus RTMP tai pirstoutuneet MP4-muodossa. Lisätietoja on artikkelissa [Azure Media Services RTMP tuki- ja Live koodauslaitteista](http://go.microsoft.com/fwlink/?LinkId=532824).
    
    Tämän vaiheen myös voi suorittaa, kun olet luonut kanavan.

1. Luo ja Käynnistä läpivienti kanavan.
1. Nouda kanavan ingest URL-osoite. 

    Ingest URL-Osoitetta käytetään live encoder virta lähettäminen kanava.
1. Noutaa kanavan esikatselu URL-osoite. 

    Tätä URL-Osoitteen avulla voit tarkistaa kanavan oikein lähetetään live stream.

3. Luo live tapahtuma tai ohjelma. 

    Azure portaalin käytettäessä live tapahtuman luomisesta luo myös sijoituksen. 
      
    >[AZURE.NOTE]Varmista, että ainakin yksi streaming varattu yksikön streaming päätepisteen, josta haluat sisällön.
1. Käynnistä tapahtuma tai ohjelma, kun olet valmis aloittamaan streaming ja arkistointi.
2. Voit myös live encoder voi tehdä Aloita mainos. Ilmoitus lisätään tulostus-muodossa.
1. Lopeta tapahtuman/ohjelman aina, kun haluat lopettaa streaming ja arkistointi tapahtuma.
1. Poista tapahtuman/ohjelman (ja halutessasi poistaa kohteen).     

>[AZURE.IMPORTANT] Lue lisätietoja käsitteitä ja huomioon otettavia seikkoja liittyvät live streaming paikallinen koodauslaitteista ja läpivienti kanavien [Live streaming luovien usean bittitaajuus virtaa paikallinen koodauslaitteista](media-services-live-streaming-with-onprem-encoders.md) .

##<a name="to-view-notifications-and-errors"></a>Näytä ilmoitukset ja virheet

Jos haluat saada ilmoituksia ja Azure portaalin tuottaa virheitä, napsauta napsauttamalla ilmaisinalueen kuvaketta.

![Ilmoitukset](./media/media-services-portal-passthrough-get-started/media-services-notifications.png)

##<a name="configure-streaming-endpoints"></a>Määritä streaming päätepisteet 

Media-palvelut on dynaaminen pakkaus, jonka avulla voit pitää usean bittitaajuus MP4s streaming seuraavissa muodoissa: MPEG VIIVA, HLS, tasainen Streaming tai HDS ilman, että sinun tarvitsee pakata huomioon nämä streaming muotoja. Dynaaminen pakkaus tarvitset vain tallentamiseen ja maksaa yhden Tallennusmuoto tiedostot sekä Media Services muodostaa ja on oikea vastaus asiakaskoneesta pyyntöjen perusteella.

Dynaaminen pakkaus hyödyntää, tarvitset vähintään yhden streaming yksikön streaming päätepisteen, josta aiot toimituksen sisältöä.  

Voit luoda ja muuttaa streaming varattu yksiköiden määrän, toimi seuraavasti:

1. Kirjaudu sisään osoitteessa [Azure portal](https://portal.azure.com/).
1. Valitse **asetukset** -ikkunassa **Virtautetun median päätepisteet**. 

2. Valitse streaming päätepisteen oletusarvo. 

    **Oletus-TIETOVIRTA PÄÄTEPISTEEN tiedot** -ikkuna.

3. Jos haluat määrittää streaming yksiköiden määrän, **Virtautetun median yksiköt** liukusäädintä.

    ![Streaming yksiköt](./media/media-services-portal-passthrough-get-started/media-services-streaming-units.png)

4. Tallenna muutokset valitsemalla **Tallenna** .

    >[AZURE.NOTE]Kaikki uudet yksiköt kohdistus voi kestää jopa 20 minuuttia.
    
##<a name="create-and-start-pass-through-channels-and-events"></a>Luo ja Käynnistä läpivienti kanavat ja tapahtumat

Kanavan on liitetty tapahtumien/ohjelmat, joiden avulla voit hallita julkaisu- ja live stream osia tallennustilaan. Kanavien hallita tapahtumia. 
    
Voit määrittää ohjelman tallennetun sisällön säilytettävien määrittämällä **Arkisto-ikkunassa** pituus tuntimäärä. Tämän arvon voi määrittää enintään 25 tuntia vähintään viiteen minuuttiin. Arkisto-ikkunassa pituus määrittää myös aika asiakkaiden enimmäismäärän voit Etsi taaksepäin nykyisestä live kohdasta. Tapahtumien suorittamisen tietyn ajanjakson päälle, mutta sisältöä, joka on välin takana ikkunan pituus jatkuvasti poistetaan. Tämän ominaisuuden arvoksi myös määrittää, kuinka kauan asiakkaan luettelot voi kasvaa.

Kuhunkin tapahtumaan on liitetty sijoituksen. Voit julkaista tapahtumaa, sinun on luotava OnDemand-locator, liitetty annetaan. Ottaa tämän locator avulla voit luoda streaming URL-osoite, voit tarjota asiakkaat.

Kanavan tukee jopa kolme samanaikaisesti käynnissä olevat tapahtumat, joten voit luoda useita arkistot samalla saapuvat muodossa. Näin voit julkaista ja arkistoida tapahtuman eri osia tarpeen mukaan. Esimerkiksi business vaatimus on arkistoon ohjelman 6 tuntia, mutta vain viimeisten 10 minuutin yleislähetys. Tämän vuoksi haluat luoda kahden samanaikaisesti käynnissä olevat ohjelmat. Yksi ohjelma on määritetty arkistoon tapahtuman 6 tuntia, mutta ohjelma ei ole julkaistu. Toinen ohjelma on määritetty arkistoida 10 minuutin ja ohjelma on julkaistu.

Ei pitäisi käyttää uudelleen live olemassa olevat tapahtumat. Sen sijaan luominen ja aloittaa uuden tapahtuman jokaisen tapahtuman.

Aloita tapahtuman, kun olet valmis aloittamaan streaming ja arkistointi. Lopeta ohjelma aina, kun haluat lopettaa streaming ja arkistointi tapahtuma. 

Voit poistaa arkistoidut sisältöä lopettaminen ja poistaa tapahtuman ja poista liitetyn kohteen. Sijoituksen ei voi poistaa, jos se on käyttämä tapahtuman; tapahtuma on poistettava ensin. 

Vaikka lopettaminen ja poistaa tapahtuman, käyttäjien olisi voi lähettää arkistoidut sisällön pyydettäessä, videona kunhan, älä poista kohteen.

Jos haluat säilyttää arkistoidut sisällön, mutta se ei ole käytettävissä streaming, poista streaming locator.

###<a name="to-use-the-portal-to-create-a-channel"></a>Käyttää portaalia kanavan luominen 

Tässä osassa esitellään läpivienti kanavan luominen **Nopea luominen** -vaihtoehdon avulla.

Saat lisätietoja läpivienti kanavien [Live streaming luovien usean bittitaajuus virtaa paikallinen koodauslaitteista kanssa](media-services-live-streaming-with-onprem-encoders.md).

1. Valitse [Azure portal](https://portal.azure.com/)Azure Media Services-tilin.
2. Valitse **asetukset** -ikkunassa **Live streaming**. 

    ![Käytön aloittaminen](./media/media-services-portal-passthrough-get-started/media-services-getting-started.png)
    
    **Live streaming** -ikkuna tulee näkyviin.

3. Valitse läpivienti kanavan luominen ja RTMP **Nopea luominen** ingest protokolla.

    **Luo A uusi KANAVA** -ikkuna.
4. Kirjoita uuden kanavan nimi ja valitse **Luo**. 

    Tämä luo läpivienti kanavan, jossa RTMP ingest protokolla.

##<a name="create-events"></a>Tapahtumien luominen

1. Valitse kanavan, johon haluat lisätä tapahtuman.
2. Paina **Live tapahtuma** -painiketta.

![Tapahtuman](./media/media-services-portal-passthrough-get-started/media-services-create-events.png)


##<a name="get-ingest-urls"></a>Hae ingest URL-osoitteet

Kun kanava luodaan, saa ingest URL-osoitteet, jotka haluat antaa reaaliaikaisia encoder. Encoder käyttää näitä URL-osoitteet kirjaimilla live muodossa.

![Luotu](./media/media-services-portal-passthrough-get-started/media-services-channel-created.png)

##<a name="watch-the-event"></a>Katso tapahtuma

Katso tapahtuman, valitse **Watch** Azure-portaalissa tai kopioi streaming URL-osoite ja valittua toisto. 
 
![Luotu](./media/media-services-portal-passthrough-get-started/media-services-default-event.png)

Live tapahtuman Hae muuntaa automaattisesti tarvittaessa sisältöön, kun pysäytetty.

##<a name="clean-up"></a>Puhdista

Saat lisätietoja läpivienti kanavien [Live streaming luovien usean bittitaajuus virtaa paikallinen koodauslaitteista kanssa](media-services-live-streaming-with-onprem-encoders.md).

- Kanavan voidaan pysäyttää vain silloin, kun kaikki tapahtumat/ohjelmat kanavalla pysäyttämisestä.  Kanavan pysäytetään, se ei maksamaan kulut. Kun haluat käynnistää uudelleen, se on sama ingest URL-osoite, joten sinun ei tarvitse määrittää oman encoder uudelleen.
- Kanavan voi poistaa vain silloin, kun kaikki live tapahtumat kanavalla on poistettu.

##<a name="view-archived-content"></a>Arkistoidut sisällön tarkasteleminen

Vaikka lopettaminen ja poistaa tapahtuman, käyttäjien olisi voi lähettää arkistoidut sisältöä pyydettäessä, videona kunhan, älä poista kohteen. Sijoituksen ei voi poistaa, jos se on käyttämä tapahtuman; tapahtuma on poistettava ensin. 

Hallittavan oman kalusto **-asetuksen** valitseminen ja sitten **resurssit**.

![Resurssit](./media/media-services-portal-passthrough-get-started/media-services-assets.png)

##<a name="next-step"></a>Seuraava vaihe

Tarkista oppimispolut Media-palveluita.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
