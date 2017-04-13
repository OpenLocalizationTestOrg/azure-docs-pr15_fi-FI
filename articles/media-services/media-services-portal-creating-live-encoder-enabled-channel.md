<properties 
    pageTitle="Tietoja live streaming Azure Media-palvelujen avulla voit luoda usean bittitaajuus virtaa Azure-portaalissa | Microsoft Azure" 
    description="Tässä opetusohjelmassa käydään läpi vaiheet, joka vastaanottaa yhden bittitaajuus live virta ja käyttää sitä usean bittitaajuus stream Azure-portaalissa kanavan luominen." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
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


#<a name="how-to-perform-live-streaming-using-azure-media-services-to-create-multi-bitrate-streams-with-the-azure-portal"></a>Tietoja live streaming Azure Media-palvelujen avulla voit luoda usean bittitaajuus virtaa Azure-portaalissa

> [AZURE.SELECTOR]
- [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [REST-OHJELMOINTIRAJAPINNALLA](https://msdn.microsoft.com/library/azure/dn783458.aspx)

Tässä opetusohjelmassa käydään läpi vaiheet, joka vastaanottaa yhden bittitaajuus live virta ja käyttää sitä usean bittitaajuus stream **kanavan** luominen.

>[AZURE.NOTE]Käsitteellinen liittyvät, jotka on otettu käyttöön live koodaus kanavien, katso lisätietoja [Live streaming luominen usean bittitaajuus virtaa Azure Media-palveluiden avulla](media-services-manage-live-encoder-enabled-channels.md).

##<a name="common-live-streaming-scenario"></a>Käytetty Live Streaming-vaihtoehto

Seuraavassa on Yleiset vaiheet yleisiä live streaming sovellusten luominen.

>[AZURE.NOTE] Tällä hetkellä live tapahtuman max suositeltava kesto on kahdeksan tuntia. Ota yhteyttä amslived Microsoft.comin, jos haluat suorittaa kanavan pitkällä aikavälillä.

1. Kameraa muodostaa yhteyttä tietokoneeseen. Käynnistä ja määrittää paikallisen live encoder, joka voi tulostaa yhden bittitaajuus stream jollakin seuraavista protokollista: RTMP, tasainen Streaming tai RTP (MPEG-TS). Lisätietoja on artikkelissa [Azure Media Services RTMP tuki- ja Live koodauslaitteista](http://go.microsoft.com/fwlink/?LinkId=532824).
    
    Tämän vaiheen myös voi suorittaa, kun olet luonut kanavan.

1. Luo ja Käynnistä kanavan. 

1. Nouda kanavan ingest URL-osoite. 

    Ingest URL-Osoitetta käytetään live encoder virta lähettäminen kanava.
1. Noutaa kanavan esikatselu URL-osoite. 

    Tätä URL-Osoitteen avulla voit tarkistaa kanavan oikein lähetetään live stream.

3. Voit luoda tapahtuman/ohjelman (eli luo myös sijoituksen). 
1. Julkaise tapahtuman (eli Luo OnDemand-locator, liitetty annetaan).  

    Varmista, että ainakin yksi streaming varattu yksikön streaming päätepisteen, josta haluat sisällön.
1. Aloita tapahtuman, kun olet valmis aloittamaan streaming ja arkistointi.
2. Voit myös live encoder voi tehdä Aloita mainos. Ilmoitus lisätään tulostus-muodossa.
1. Lopeta tapahtuman aina, kun haluat lopettaa streaming ja arkistointi tapahtuma.
1. Tapahtuman poistaminen (ja halutessasi poistaa kohteen).   

##<a name="in-this-tutorial"></a>Tässä opetusohjelmassa

Tässä opetusohjelmassa Azure portaalin käytetään suorittamaan seuraavia tehtäviä: 

2.  Määritä streaming päätepisteet.
3.  Luo kanava suorittamiseen live koodaus on otettu käyttöön.
1.  Jotta voit toimittaa sen live encoder Ingest URL-Osoitteen hakeminen Live encoder ingest virta kanavan kyselyjä käyttämällä tätä URL-Osoitetta. .
1.  Luoda tapahtuman/ohjelman (ja sijoituksen)
1.  Julkaista kohteen ja poistaa streaming URL-osoitteet  
1.  Toistaa sisältöä 
2.  Puhdistaminen

##<a name="prerequisites"></a>Edellytykset

Seuraavassa on suorittamiseen tarvittava opetusohjelman.

- Jotta voit suorittaa tässä opetusohjelmassa, sinun on Azure-tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).
- Media Services-tilin. Media Services-tilin luominen-kohdassa [Luo tili](media-services-portal-create-account.md).
- Verkkokamera ja encoder, voit lähettää yksittäisen bittitaajuus live muodossa.

##<a name="configure-streaming-endpoints"></a>Määritä streaming päätepisteet 

Media-palvelut on dynaaminen pakkaus, jonka avulla voit pitää usean bittitaajuus MP4s streaming seuraavissa muodoissa: MPEG VIIVA, HLS, tasainen Streaming tai HDS, eikä sinun tarvitse uudelleen pakkaaminen huomioon nämä streaming muotoja. Dynaaminen pakkaus tarvitset vain tallentamiseen ja maksaa yhden Tallennusmuoto tiedostot sekä Media Services luominen ja yhteyshenkilönä oikea vastaus asiakaskoneesta pyyntöjen perusteella.

Dynaaminen pakkaus hyödyntää, tarvitset vähintään yhden streaming yksikön streaming päätepisteen, josta aiot toimituksen sisältöä.  

Voit luoda ja muuttaa streaming varattu yksiköiden määrän, toimi seuraavasti:

1. Kirjaudu sisään osoitteessa [Azure portal](https://portal.azure.com/) ja AMS-tili.
1. Valitse **asetukset** -ikkunassa **Virtautetun median päätepisteet**. 

2. Valitse streaming päätepisteen oletusarvo. 

    **Oletus-TIETOVIRTA PÄÄTEPISTEEN tiedot** -ikkuna.

3. Jos haluat määrittää streaming yksiköiden määrän, **Virtautetun median yksiköt** liukusäädintä.

    ![Streaming yksiköt](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-streaming-units.png)

4. Tallenna muutokset valitsemalla **Tallenna** .

    >[AZURE.NOTE]Kaikki uudet yksiköt kohdistus voi kestää jopa 20 minuuttia.

##<a name="create-a-channel"></a>KANAVAN luominen

1. [Azure portal](https://portal.azure.com/)Valitse Media Services ja valitse Media Services-tilin nimi.
2. Valitse **Live Streaming**.
3. Valitse **Mukautettu luominen**. Voit luoda kanavaan, joka on otettu käyttöön live koodaus tämän asetuksen avulla.

    ![Kanavan luominen](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel.png)
    
4. Valitse **asetukset**.
    
    1.  Valitse **Live koodaus** kanavien tyyppi. Tällaista määrittää, että haluat luoda kanavaan, joka on otettu käyttöön live koodaus. Tämä tarkoittaa sitä, saapuvat yksittäisen bittitaajuus stream lähetetään kanavaan ja koodattu kyselyjä usean bittitaajuus stream määritetyn live encoder asetusten avulla. Lisätietoja on artikkelissa [Live streaming luominen usean bittitaajuus virtaa Azure Media-palveluiden avulla](media-services-manage-live-encoder-enabled-channels.md). Valitse OK.
    2. Määritä kanavan nimi.
    3. Valitse näytön alareunassa OK.
    
5. Valitse **Ingest** -välilehti.

    1. Tällä sivulla voit valita streaming protokolla. **Live koodaus** tyyppi-kanava-kelvollinen protokolla-vaihtoehdot ovat seuraavat:
        
        - Yksittäinen bittitaajuus Fragmented MP4 (tasainen Streaming)
        - Yksittäisen bittitaajuus RTMP
        - RTP (MPEG-TS): MPEG-2 Transport Stream RTP päälle.
        
        Katso yksityiskohtainen kuvaus siitä, että kunkin protokolla, [Live streaming luominen usean bittitaajuus virtaa Azure Media-palveluiden avulla](media-services-manage-live-encoder-enabled-channels.md).
    
        Kun kanava protokolla-asetus ei voi muuttaa tai liittyvät tapahtumat/ohjelmia on käynnissä. Jos asetat protokollia, sinun on luotava erillinen kanavien kunkin streaming-protokollaa.  

    2. Voit käyttää IP-rajoitus ingest. 
    
        Voit määrittää IP-osoitteet, jotka saavat ingest tähän kanavaan videon. Sallitut IP-osoitteiden voidaan määrittää yksittäisen IP-osoite (esimerkiksi 10.0.0.1), käyttämällä IP-osoite ja CIDR aliverkkopeite (kuten 10.0.0.1/22) IP-alueen tai IP-osoite ja pisteviiva desimaaleja aliverkkopeite IP-alueen (esimerkiksi "10.0.0.1(255.255.252.0)').

        Jos IP-osoitteita ei ole määritetty ja että ei ole säännön määritys ei ole IP-osoite sallitaan. Jos mikä tahansa IP-osoite, säännön luominen ja määrittäminen 0.0.0.0/0.

6. **Esikatselu** -välilehdessä Käytä IP-rajoitus, valitse Esikatselu.
7. Määritä koodauksen esiasetus **koodaus** -välilehti. 

    Tällä hetkellä vain järjestelmän esiasetus, voit valita on **oletusarvoinen 720 p-näppäinyhdistelmää**. Jos haluat määrittää mukautetun esiasetus, Avaa Microsoft-tuen lippu. Kirjoita nimi, voit luoda esiasetus. 

>[AZURE.NOTE] Tällä hetkellä kanavan Käynnistä saattaa kestää enimmillään 30 minuuttia. Kanavan Palauta voi kestää jopa 5 minuuttia.

Kun olet luonut kanavaa, voit valitsemalla Valitse kanavan ja valitsemalla **asetukset** , missä voit katsella kanavat-määrityksiä. 

Lisätietoja on artikkelissa [Live streaming luominen usean bittitaajuus virtaa Azure Media-palveluiden avulla](media-services-manage-live-encoder-enabled-channels.md).


##<a name="get-ingest-urls"></a>Hae ingest URL-osoitteet

Kun kanava luodaan, saa ingest URL-osoitteet, jotka haluat antaa reaaliaikaisia encoder. Encoder käyttää näitä URL-osoitteet kirjaimilla live muodossa.

![ingesturls](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-ingest-urls.png)


##<a name="create-and-manage-events"></a>Voit luoda ja hallita tapahtumat

###<a name="overview"></a>Yleiskatsaus

Kanavan on liitetty tapahtumien/ohjelmat, joiden avulla voit hallita julkaisu- ja live stream osia tallennustilaan. Kanavien hallita tapahtumia/ohjelmat. Kanavan ja ohjelma yhteyden muistuttaa perinteinen media, jossa kanavan on vakio stream sisällön ja ohjelma on määritetty kyseisen kanavan joitakin ajoitetun tapahtuma.

Voit määrittää tuntimäärä, jotka haluat säilyttää tapahtuman tallennetun sisällön määrittämällä **Arkisto-ikkunassa** pituus. Tämän arvon voi määrittää enintään 25 tuntia vähintään viiteen minuuttiin. Arkisto-ikkunassa pituus määrittää myös aika asiakkaiden enimmäismäärän voit Etsi taaksepäin nykyisestä live kohdasta. Tapahtumien suorittamisen tietyn ajanjakson päälle, mutta sisältöä, joka on välin takana ikkunan pituus jatkuvasti poistetaan. Tämän ominaisuuden arvoksi myös määrittää, kuinka kauan asiakkaan luettelot voi kasvaa.

Kuhunkin tapahtumaan on liitetty sijoituksen. Voit julkaista tapahtuman sinun on luotava OnDemand-locator, liitetty annetaan. Ottaa tämän locator avulla voit luoda streaming URL-osoite, voit tarjota asiakkaat.

Kanavan tukee jopa kolme samanaikaisesti käynnissä olevat tapahtumat, joten voit luoda useita arkistot samalla saapuvat muodossa. Näin voit julkaista ja arkistoida tapahtuman eri osia tarpeen mukaan. Esimerkiksi business vaatimus on arkistoon tapahtuman 6 tuntia, mutta vain viimeisten 10 minuutin yleislähetys. Tätä varten on luotava kaksi samanaikaisesti käynnissä tapahtuma. Tapahtuma on määritetty arkistoida tapahtuman 6 tuntia, mutta ohjelma ei ole julkaistu. Toinen tapahtuma on määritetty arkistoida 10 minuutin ja ohjelma on julkaistu.

Ei pitäisi käyttää uudelleen uusille tapahtumille olemassa olevat ohjelmat. Sen sijaan luominen ja aloittaa uuden ohjelman jokaisen tapahtuman.

Tapahtuman/ohjelman käynnistäminen, kun olet valmis aloittamaan streaming ja arkistointi. Lopeta tapahtuman aina, kun haluat lopettaa streaming ja arkistointi tapahtuma. 

Voit poistaa arkistoidut sisältöä lopettaminen ja poistaa tapahtuman ja poista liitetyn kohteen. Sijoituksen ei voi poistaa, jos sitä käytetä tapahtuman; tapahtuma on poistettava ensin. 

Vaikka lopettaminen ja poistaa tapahtuman, käyttäjien olisi voi lähettää arkistoidut sisältöä pyydettäessä, videona kunhan, älä poista kohteen.

Jos haluat säilyttää arkistoidut sisältöä, mutta ei ole käytettävissä streaming, poista streaming locator.

###<a name="createstartstop-events"></a>Luo, Aloita/Lopeta-tapahtumat

Kun sinulla on kanavan kyselyjä juoksutus stream streaming tapahtuman voit aloittaa luomalla resurssi, ohjelma ja Streaming Locator. Tämä arkistoida virta ja käytettäväksi tarkastelijat Streaming päätepisteen kautta. 

Aloita tapahtuman kahdella tavalla: 

1. Paina **Live tapahtuman** voit lisätä uuden tapahtuman **kanava** -sivu.

    Määritä: tapahtuman nimi, resurssi nimi, arkisto-ikkuna ja salaus-asetusta.
    
    ![createprogram](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-program.png)
    
    Jos olet jättänyt **Julkaise live tapahtuman nyt** valittuna, tapahtuman JULKAISUN URL-osoitteet Hae luodaan.
    
    **Käynnistä-painiketta**, voit painaa aina, kun haluat käyttää tapahtuma.

    Kun olet aloittanut tapahtuman, voit painaa alkavan sisällön **Seuranta** .

2. Voit vaihtoehtoisesti käyttää pikakuvakkeen ja paina **Go Live** painike **kanava** -sivu. Tämä luo oletusarvoisesti resurssi, ohjelma ja Streaming Locator.

    Tapahtuman nimi on **oletusarvon** ja arkisto-ikkuna on määritetty kahdeksan tuntia.

Voit katsoa julkaistun tapahtuman **Live tapahtuma** -sivulta. 

Jos valitset **Käytöstä ilman**, se lopettaa kaikki live tapahtumat. 


##<a name="watch-the-event"></a>Katso tapahtuma

Katso tapahtuman, valitse **Watch** Azure-portaalissa tai kopioi streaming URL-osoite ja valittua toisto. 
 
![Luotu](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-play-event.png)

Live tapahtuman muuntaa automaattisesti tarvittaessa sisältöä milloin keskeyttää tapahtumat.

##<a name="clean-up"></a>Puhdista

Jos tehdään streaming tapahtumien ja haluat järjestää resurssien valmisteltu aiemmin, toimi seuraavasti.

- Lopeta valitseminen virta encoder kohteesta.
- Lopeta kanava. Kanavan pysäytetään, se ei ole maksamaan kulut. Kun haluat käynnistää uudelleen, se on sama ingest URL-osoite, joten sinun ei tarvitse määrittää oman encoder uudelleen.
- Voit lopettaa Streaming-päätepisteen paitsi, jos haluat jatkaa live tapahtuman tarvittaessa muodossa kuin arkisto. Kanavan on pysäytetty-tilaan, jos se ei ole maksamaan kulut.
  
##<a name="view-archived-content"></a>Arkistoidut sisällön tarkasteleminen

Vaikka lopettaminen ja poistaa tapahtuman, käyttäjien olisi voi lähettää arkistoidut sisällön pyydettäessä, videona kunhan, älä poista kohteen. Sijoituksen ei voi poistaa, jos se on käyttämä tapahtuman; tapahtuma on poistettava ensin. 

Hallittavan oman kalusto **-asetuksen** valitseminen ja sitten **resurssit**.

![Resurssit](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-assets.png)

##<a name="considerations"></a>Huomioon otettavia seikkoja

- Tällä hetkellä live tapahtuman max suositeltava kesto on kahdeksan tuntia. Ota yhteyttä amslived Microsoft.comin, jos haluat suorittaa kanavan pitkällä aikavälillä.
- Varmista, että ainakin yksi streaming varattu yksikön streaming päätepisteen, josta haluat sisällön.


##<a name="next-step"></a>Seuraava vaihe

Tarkista oppimispolut Media-palveluita.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
