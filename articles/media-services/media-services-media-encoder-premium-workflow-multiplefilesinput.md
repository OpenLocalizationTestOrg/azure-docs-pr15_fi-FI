<properties
    pageTitle="Useita syöte- ja osan ominaisuuksien käyttäminen Premium Encoder | Microsoft Azure"
    description="Tässä ohjeaiheessa kerrotaan, miten voit käyttää setRuntimeProperties useita syötteen tiedostojen käyttäminen ja välittää mukautettuja tietoja Media Encoder Premium työnkulun media-suoritin."
    services="media-services"
    documentationCenter=""
    authors="xpouyat"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"  
    ms.author="xpouyat;anilmur;juliako"/>

# <a name="using-multiple-input-files-and-component-properties-with-premium-encoder"></a>Useita syöte- ja osan ominaisuuksien käyttäminen Premium Encoder

## <a name="overview"></a>Yleiskatsaus

Skenaariot, jossa voit voi mukauttaa osan ominaisuuksia, Määritä ClipArt-luettelossa XML-sisällön tai useita syötteen tiedostojen lähettäminen, kun lähetät **Media Encoder Premium työnkulun** media-suoritin tehtävään on. On joitakin esimerkkejä:

- Teksti, jossa video ja määrittämällä tekstiarvo (esimerkiksi nykyinen päivämäärä) suorituksen kunkin syötteen videon.
- Mukauttaminen ClipArt luettelon XML (Jos haluat määrittää yhden tai usean lähdetiedostot kanssa tai ilman rajaamisen jne.).
- Kun video on koodattu videon syöte, jossa logokuva.

Jos haluat sallia tietää, että olet muuttamassa työnkulun joitakin ominaisuuksia, kun luot tehtävän tai useita syötteen tiedostojen lähettäminen, sinun on käytettävä määritysten merkkijonon **Media Encoder Premium työnkulun** , joka sisältää **setRuntimeProperties** ja/tai **transcodeSource**. Tässä ohjeaiheessa kerrotaan, miten voit käyttää niitä.


## <a name="configuration-string-syntax"></a>Määritysten merkkijonosyntaksi

Määritysten merkkijonon koodauksen tehtävän määrittämisestä käyttää XML-tiedosto, joka näyttää tältä:

    <?xml version="1.0" encoding="utf-8"?>
    <transcodeRequest>
      <transcodeSource>
      </transcodeSource>
      <setRuntimeProperties>
        <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
      </setRuntimeProperties>
    </transcodeRequest>


Seuraavassa on C#-koodin, joka lukee XML-määritykset tiedostosta ja välittää sen projektin tehtävän:

    XDocument configurationXml = XDocument.Load(xmlFileName);
    IJob job = _context.Jobs.CreateWithSingleTask(
                                                  "Media Encoder Premium Workflow",
                                                  configurationXml.ToString(),
                                                  myAsset,
                                                  "Output asset",
                                                  AssetCreationOptions.None);


## <a name="customizing-component-properties"></a>Osan ominaisuuksien mukauttaminen  

### <a name="property-with-a-simple-value"></a>Yksinkertainen arvo ominaisuus
Joissakin tapauksissa kannattaa käyttää mukauttamiseen ja työnkulku-tiedosto, joka voidaan suorittaa Media Encoder Premium työnkulun suorittaminen osan ominaisuus.

Oletetaan, että suunnittelet työnkulun päällekkäiset tekstin, videoita ja teksti (esimerkiksi nykyinen päivämäärä) on tarkoitus määrittää suorituksen aikana. Voit tehdä tämän lähettämällä tekstin koodauksen tehtävästä voidaan asettaa tekstin kerros-osan ominaisuudelle uusi arvo. Voit muuttaa muita ominaisuuksia (esimerkiksi paikkaa tai väriä kerroksissa, bittitaajuus AVC encoder jne.) työnkulun osan se.

**setRuntimeProperties** käytetään ohittaa ominaisuuden työnkulun osat.

Esimerkki:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
          <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
          <property propertyPath="Optional Overlay/Overlay/filename" value="MyLogo.png"/>
          <property propertyPath="Optional Text Overlay/Text To Image Converter/text" value="Today is Friday the 13th of May, 2016"/>
      </setRuntimeProperties>
    </transcodeRequest>


### <a name="property-with-an-xml-value"></a>XML-arvolla ominaisuus

Määritä ominaisuus, joka odottaa XML-arvo, kapseloida käyttämällä `<![CDATA[ and ]]>`.

Esimerkki:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

>[AZURE.NOTE]Varmista, että ei, jos haluat lisätä rivinvaihdon palautuksen jälkeen `<![CDATA[`.


### <a name="propertypath-value"></a>propertyPath arvo

Edellisessä esimerkissä propertyPath on "/ mediatiedostoa syöte/tiedostonimi" tai "/ inactiveTimeout" tai "clipListXml".
Tämä on yleensä osan nimi ja valitse ominaisuuden nimi. Polku voi olla enemmän tai vähemmän tasoja, kuten "/ primarySourceFile" (koska se on työnkulun ylimmällä tasolla) tai "/ videon käsittely ja graafiset kerros/läpikuultavuus" (koska kerros on osa ryhmää).    

Voit tarkistaa polku ja ominaisuuden käyttämällä toiminto-painiketta, joka on heti kunkin ominaisuuden vieressä. Voit tämä toiminto-painiketta ja valitse **Muokkaa**. Tämä näyttää nimi-ominaisuuden ja heti sen nimitilan yläpuolella.

![Toiminnon/muokkaaminen](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture6_actionedit.png)

![Ominaisuus](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture7_viewproperty.png)

## <a name="multiple-input-files"></a>Useita syötteen tiedostoja

Voit lähettää **Media Encoder Premium työnkulun** kunkin tehtävän edellyttää kahta kalusto:

- Ensimmäinen on *Työnkulun resurssi* , joka sisältää työnkulun. Voit suunnitella [Työnkulun suunnittelutyökalun](media-services-workflow-designer.md)työnkulun tiedostot.
- Toinen on *Media resurssi* , joka sisältää, jonka haluat koodata mediatiedostoa.

Kun lähetät useita mediatiedostojen **Media Encoder Premium työnkulun** encoder, koskevat seuraavat rajoitukset:

- Kaikki mediatiedostot on oltava sama *Media resurssi*. Useita mediasisältöä ei tueta.
- Sinun on määritettävä ensisijainen tiedosto Media-resurssi (Ihannetapauksessa tämä on ensisijainen videotiedostoa, joka encoder pyydetään käsittelemään).
- On tarpeen välittää kokoonpanotietoja, joka sisältää suorittimen **setRuntimeProperties** ja/tai **transcodeSource** elementin.
  - **setRuntimeProperties** käytetään ohittaa filename-ominaisuuden tai toisen työnkulun osat-ominaisuutta.
  - ClipArt-luettelossa XML-sisällön käytetään **transcodeSource** .

Työnkulun yhteydet:

 - Jos käytät yksi tai useita Media tiedoston Input-osat ja suunnitelman **setRuntimeProperties** avulla voit määrittää tiedoston nimeä, valitse yhteyttä ensisijainen tiedoston osa PIN-tunnuksen ne. Varmista, että ei ole yhteyttä ensisijainen tiedosto-objekti- ja Media tiedoston tulo tai tulot välillä.
 - Jos haluat käyttää ClipArt luettelon XML- ja yksi tietolähde-osa, sitten voi yhdistää molemmat.

![Liity ensisijainen lähdetiedoston Media tiedoston syöte](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture0_nopin.png)

*Yhteyttä ei ole ensisijainen tiedostosta Media tiedoston Input-komponentteja, jos käytät setRuntimeProperties filename-ominaisuuden määrittäminen.*

![ClipArt-luettelosta XML Clip luettelon lähteen yhteys](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture1_pincliplist.png)

*Voit muodostaa tietolähde ClipArt-luettelossa XML ja käyttää transcodeSource.*


### <a name="clip-list-xml-customization"></a>ClipArt-luettelossa XML mukauttaminen
Voit määrittää ClipArt-luettelossa XML työnkulun tietomäärää käyttämällä **transcodeSource** määritysten merkkijonon XML. Tämä edellyttää ClipArt-luettelossa XML PIN-tunnuksen edellyttää yhteyttä työnkulun tietolähde-osa.

    <?xml version="1.0" encoding="utf-16"?>
      <transcodeRequest>
        <transcodeSource>
          <clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>video-part1.mp4</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>video-part1.mp4</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
          </clipList>
        </transcodeSource>
        <setRuntimeProperties>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

Jos haluat määrittää tämän ominaisuuden avulla voit nimeä tulosteen tiedostot käyttäen 'lausekkeita,' /primarySourceFile, sitten Suosittelemme kulkeva ClipArt-luettelossa XML kuin ominaisuuden *jälkeen* /primarySourceFile-ominaisuutta, vältä /primarySourceFile-asetus ohittaa leikeluettelon.

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="c:\temp\start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>c:\temp\start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>c:\temp\start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

Lisää kehyksen tarkka rajaaminen: kanssa

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <trim>
                  <inPoint fps="25">00:00:05:24</inPoint>
                  <outPoint fps="25">00:00:10:24</outPoint>
                </trim>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
               <trim>
                  <inPoint fps="25">00:00:05:24</inPoint>
                  <outPoint fps="25">00:00:10:24</outPoint>
                </trim>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>


## <a name="example"></a>Esimerkki

Harkitse esimerkki, jossa haluat logon kuvan, videon Input päälle, kun video on koodattu. Tässä esimerkissä syötteen videon nimi "MyInputVideo.mp4" on ja logon nimeltä "MyLogo.png". Olisi toimimalla seuraavasti:

- Luo työnkulun resurssi työnkulun-tiedoston (katso seuraava esimerkki).
- Luo Media resurssi, joka sisältää kaksi tiedostoa: MyInputVideo.mp4 ensisijainen tiedosto ja MyLogo.png.
- Tehtävän lähettäminen yllä syötteen varoja Media Encoder Premium työnkulun media-suoritin ja määritä seuraavat määritykset-merkkijono.

Määritys:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
          <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
          <property propertyPath="Media File Input Logo/filename" value="MyLogo.png" />
        </setRuntimeProperties>
      </transcodeRequest>


Yllä olevassa esimerkissä videotiedostoa nimi lähetetään Media tiedoston Input-osa ja primarySourceFile-ominaisuus. Logotiedoston nimi lähetetään toiseen Media-tiedosto syötteen, joka on liitetty kuvan kerros-osa.

>[AZURE.NOTE]Videon tiedostonimi lähetetään primarySourceFile-ominaisuus. Tämä on käyttää tätä ominaisuutta työnkulun etsimisen käyttäen lausekkeita, kuten oikea tulosteen tiedostonimi.


### <a name="step-by-step-workflow-creation-that-overlays-a-logo-on-top-of-the-video"></a>Vaiheittaiset ohjeet työnkulun luominen kyseisen päällekkäiset a logon videon päälle     

Voit luoda työnkulun, joka otetaan syötteeksi kaksi tiedostoa seuraavasti: videon ja kuvan. Se merkintäkerroksen kuva videon päälle.

Avaa **Työnkulun suunnittelussa** ja valitse **Tiedosto** > **Uuden työtilan** > **Muuta Sinikopio**.

Uusi työnkulku näkyy kolme osaa:

- Ensisijainen lähdetiedosto
- Leikeluettelon XML
- Tulosteen tiedoston/resurssi  

![Uusi koodauksen työnkulku](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture9_empty.png)

*Uusi koodauksen työnkulku*


Jos haluat hyväksyä syötteen mediatiedosto, Aloita Media tiedoston syötteen komponentin lisääminen. Jos haluat lisätä osan työnkulun, etsi se säilöön hakuruutuun ja suunnittelu-ruutuun haluamasi tapahtuma vetää.

Lisää seuraavaksi videotiedostoa suunnittelun työnkulkua käytetään. Voit tehdä tämän työnkulun suunnittelutyökalun Tausta-ruudussa ja etsimällä oikeanpuoleisen ominaisuusrivin ensisijainen lähdetiedosto-ominaisuus. Napsauta kansiokuvaketta ja valitse haluamasi videotiedostoa.

![Ensisijainen lähde](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture10_primaryfile.png)

*Ensisijainen lähde*


Määritä seuraavaksi videotiedostoa Media tiedoston Input-osassa.   

![Median tiedoston lähde](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture11_mediafileinput.png)

*Median tiedoston lähde*


Heti, kun tämä on valmis, Media tiedoston Input-osan Tarkasta tiedosto ja täytä sen tulosteen PIN vastaamaan tiedosto, jonka se tarkastaa.

Seuraavaksi voit lisätä "Video tietojen tyypin Updater" Voit määrittää värin tilan Rec.709. Lisää "Videon muodossa muuntimen", joka on määritetty tietojen asettelun tai asettelun tyyppi = määritettäviä tästä käytetään. Videovirtaa muunnetaan muotoa, jota voidaan ottaa lähteenä kerros-osan.

![videon tietojen tyypin Updater ja muunnin](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter.png)

*Videon tietojen tyypin Updater ja muunnin*

![Asettelutyyppi = määritettäviä tästä käytetään](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter2.png)

*Asettelutyyppi on määritettävä tästä käytetään*

Seuraavaksi Lisää Video päällekkäin-osa ja Yhdistä (pakkaamattomat) videon PIN-tunnus (pakkaamattomat) videon PIN-tunnus, syöttö media-tiedosto.

Lisää toinen Media tiedoston syötteen (lataamaan logotiedosto), valitsemalla tämä osa ja nimeksi "Media tiedoston syötteen logon" ja valitse tiedosto-ominaisuuden kuva (esimerkiksi .png-tiedosto). Yhdistä pakkaamattomat kuva PIN-tunnus kerros pakkaamattomat kuva-tunnuksesi.

![Sijoita osa ja kuva lähde](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture13_overlay.png)

*Sijoita osa ja kuva lähde*


Jos haluat muuttaa videon logon sijainti (esimerkiksi, haluat ehkä aseta se 10 prosenttia luettelosta videon vasemmassa yläkulmassa), poista "Manuaalinen syöte"-valintaruudun valinta. Voit tehdä tämän, kun käytät Media-tiedosto syötteen antamaan logotiedosto kerros-osaan.

![Sijainnin asettaminen päällekkäin](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture14_overlay_position.png)

*Sijainnin asettaminen päällekkäin*


Koodata videovirtaa, H.264 lisääminen suunnittelutyökalun pinta AVC videon Encoder ja AAC encoder osat. Yhdistä PIN.
AAC encoder määrittäminen ja valitse Audio muodon muuntaminen/esiasetus: 2.0 (L, R).

![Ääni- ja koodauslaitteista](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture15_encoders.png)

*Ääni- ja koodauslaitteista*


Nyt **ISO Mpeg-4 multiplekseri** ja **Tuloste** -osien lisääminen ja yhteyden PIN esitetyllä.

![MP4 multiplekseri ja tuloste](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture16_mp4output.png)

*MP4 multiplekseri ja tuloste*


Sinun on määritettävä kohdetiedosto nimi. **Tiedoston tulosteen** osa ja muokata tiedostoa lauseke:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_withoverlay.mp4

![Tiedoston Tulostenimi](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture17_filenameoutput.png)

*Tiedoston Tulostenimi*

Voit suorittaa työnkulun paikallisesti tarkistamaan, että se toimii oikein.

Kun se on tehty, voit suorittaa sen Azure Media Services-palveluissa.

Valmistele ensin Azure Media Services annetaan kaksi tiedostoja ei: videotiedostoa ja logo. Voit tehdä tämän käyttämällä .NET- tai REST API. Voit tehdä tämän myös käyttämällä Azure portal tai [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).

Tässä opetusohjelmassa näytetään, miten voit hallita resurssien AMSE. Voit lisätä tiedostoja sijoituksen kahdella tavalla:

- Luo paikalliseen kansioon, kopioi kaksi tiedostoa, ja vetää ja pudottaa kansio, johon **resurssi** -välilehti.
- Lataa kuin sijoituksen videotiedostoa, resurssi-tiedot, siirry tiedostot-välilehti ja Lataa useita tiedostoja (logo).

>[AZURE.NOTE]Varmista, että voit määrittää ensisijainen tiedosto resurssi (ensisijainen videotiedostoa).

![AMSE resurssi-tiedostot](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture18_assetinamse.png)

*AMSE resurssi-tiedostot*


Valitse kohteen ja valitse koodata Premium Encoder kanssa. Lataa työnkulku ja valitse se.

Välittää tietoja suorittimen-painiketta ja lisää seuraavat XML runtime-ominaisuuksien määrittäminen:

![Valitse AMSE Encoder Premium](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture19_amsepremium.png)

*Valitse AMSE Encoder Premium*


Liitä XML-tietoja. Sinun täytyy määrittää videotiedostoa nimen Media tiedoston syöttö-ja primarySourceFile. Määritä nimi nimi-logon liian.

    <?xml version="1.0" encoding="utf-16"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
          <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

![setRuntimeProperties](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture20_amsexmldata.png)

*setRuntimeProperties*


Jos .NET SDK avulla voit luoda ja suorittaa tehtävän XML-tietoja on siirrettävä määritys-merkkijonona.

    public ITask AddNew(string taskName, IMediaProcessor mediaProcessor, string configuration, TaskOptions options);

Kun työ on valmis, tulostus-resurssi MP4-tiedosto näyttää kerros!

![Videon päälle](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture21_resultoverlay.png)

*Videon päälle*


Voit ladata mallityönkulku [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).


## <a name="see-also"></a>Katso myös

- [Esittely Premium koodaus Azure Media-palvelut](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

- [Käyttämisestä Azure Media Services Premium koodaus](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

- [Koodaus tarvittaessa sisältöä Azure Media-palvelujen kanssa](media-services-encode-asset.md#media_encoder_premium_workflow)

- [Media Encoder Premium työnkulun muotoja ja pakkauksenhallinnat](media-services-premium-workflow-encoder-formats.md)

- [Työnkulun mallitiedostojen](https://github.com/AzureMediaServicesSamples/Encoding-Presets/tree/master/VoD/MediaEncoderPremiumWorkfows)

- [Azure Media Services Explorer-työkalu](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
