<properties 
    pageTitle="Avaa lähde Media Framework tasainen Streaming laajennus" 
    description="Opettele käyttämään Adobe Avaa lähde Media Framework Azure Media Services tasainen Streaming-laajennus." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


# <a name="how-to-use-the-microsoft-smooth-streaming-plugin-for-the-adobe-open-source-media-framework"></a>Opi käyttämään Microsoft-tasainen Streaming laajennuksen Adobe Avaa lähde Media Framework

##<a name="overview"></a>Yleiskatsaus


Avaa lähde Media Framework 2.0 (SS, OSMF) Microsoft tasainen Streaming-laajennus laajentaa OSMF oletusarvo-ominaisuuksia ja lisää Microsoft tasainen Streaming sisällön toistaminen uusille ja nykyisille OSMF pelaajat. Laajennus Lisää tasainen Streaming toisto-ominaisuuksien myös Stroboskoopin median toiston (SMP) avulla.

SS OSMF sisältää laajennuksen kaksi versiota:

- Staattinen tasainen Streaming ‑laajennuksen OSMF (.swc)

- Dynaaminen tasainen Streaming ‑laajennuksen OSMF (.swf)

Tämän asiakirjan oletetaan, että lukija on yleinen kokemusta OSMF ja OSMF laajennukset. Lisätietoja OSMF artikkelissa ohjeista [virallinen OSMF sivusto](http://osmf.org/).

###<a name="smooth-streaming-plugin-for-osmf-20"></a>Tasainen virtautetun median laajennuksen OSMF 2.0

Laajennus tukee ladataan ja tarvittaessa tasainen Streaming sisällön toisto seuraavia ominaisuuksia:

- Tarvittaessa tasainen Streaming toisto (toista, tauko, haku, Lopeta)
- Live tasainen Streaming toisto (toista)
- Live DVR Funktiot (Keskeytä, haku, DVR toiston, siirry-to-Live)
- Videon pakkauksenhallinnat - H.264 tuki
- Äänen pakkauksenhallinnat - AAC tuki
- Useita äänen kielen vaihtaminen OSMF valmiin ohjelmointirajapinnan kanssa
- Maks toiston laatu valinnan OSMF valmiin API
- Sivuvaunun suljettu kuvatekstit OSMF kuvatekstit laajennuksen kanssa
- Adobe&reg; Flash&reg; Player 11.4 tai uudempi versio.
- Tämä versio tukee vain OSMF 2.0.

## <a name="supported-features-and-known-issues"></a>Tuetut ominaisuudet ja tunnetut ongelmat

Tuetut ominaisuudet täydellinen luettelo on ei-tuettuja ominaisuuksia ja tunnetuista ongelmista on viittaavat [tämän asiakirjan](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf).


## <a name="loading-the-plugin"></a>Laajennuksen lataaminen
OSMF laajennukset voi ladata nimipalvelimet (kääntämisen yhteydessä) tai dynaamisesti (suorituksen). OSMF ladattavaksi tasainen Streaming-laajennus on dynaaminen-ja staattinen.

- Staattinen ladataan: lataamaan nimipalvelimet staattinen kirjasto (SWC)-tiedosto on pakollinen. Staattinen laajennukset on lisätty viitteenä projekteja ja Yhdistä lopullinen kohdetiedosto sisällä kääntämisen yhteydessä.

- Dynaaminen lataaminen: lataamaan dynaamisesti esikoostettu (SWF)-tiedosto on pakollinen. Dynaaminen laajennukset ladataan suorituksen aikana ja projektin tulos ei sisällytetä. (Käännetty tulos) Dynaaminen laajennukset voi ladata HTTP- ja tiedoston protokollia.

Saat lisätietoja staattinen ja dynaaminen lataaminen virallinen [OSMF laajennuksen sivun](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).

###<a name="ss-for-osmf-static-loading"></a>SS OSMF staattinen lataamista varten
Alla koodikatkelman voit ladata SS laajennus OSMF nimipalvelimet ja toisto basic OSMF MediaFactory luokan avulla. Ennen kuin lisäät SS OSMF koodin, varmista, että projektin viittauksissa "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc" staattinen laajennus.

```
package 
{
    
    import com.microsoft.azure.media.AdaptiveStreamingPluginInfo;
    
    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    
    
    
    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;
        

        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;

            initMediaPlayer();

        }
    
        private function initMediaPlayer():void
        {
        
            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);
            _mediaPlayerSprite.scaleMode = ScaleMode.NONE;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            //Adds the container to the stage
            addChild(_mediaPlayerSprite);
            
            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();
            
            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );
            
            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  
            
        }
        
        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            
            pluginResource = new PluginInfoResource(new AdaptiveStreamingPluginInfo( )); 
            _mediaFactory.loadPlugin( pluginResource ); 
        }
        
        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.
            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.
        loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        
        }
        
        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }
        
        
        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;
            
            state =  event.state;
            
            switch (state)
            {
                case MediaPlayerState.LOADING: 
                    
                    // A new source is started to load.
                    
                    break;
                
                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 
                    
                    break;
                
                case MediaPlayerState.BUFFERING :
                    
                    break;
                
                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }
        
        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }
        
        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.
            
            var resource:URLResource= new URLResource( sourceURL );
            
            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     
        
    }
}
```


###<a name="ss-for-osmf-dynamic-loading"></a>SS OSMF dynaaminen lataamista varten

Alla koodikatkelman esitetään, kuinka voit ladata SS laajennus OSMF dynaamisesti ja toistaa käyttää basic videon OSMF MediaFactory-luokan avulla. Ennen kuin lisäät OSMF koodin SS, kopioi "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" dynaaminen laajennus project-kansioon, jos haluat ladata tiedoston-protokollan avulla tai kopioida verkkopalvelimeen HTTP lataamista varten. Ei ole tarpeen, jos haluat sisällyttää projektiviittaukset "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc".

 
{pakkaaminen
    
    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    import flash.events.Event;
    import flash.system.Capabilities;

    
    //Sets the size of the SWF
    
    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;
        
        
        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;
            initMediaPlayer();
        }
        
        private function initMediaPlayer():void
        {
            
            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);

            //Adds the container to the stage
            addChild(_mediaPlayerSprite);
            
            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();
            
            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );
            
            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  
            
        }
        
        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            var adaptiveStreamingPluginUrl:String;

            // Your dynamic plugin web server needs to host a valid crossdomain.xml file to allow loading plugins.

            adaptiveStreamingPluginUrl = "http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf";
            pluginResource = new URLResource(adaptiveStreamingPluginUrl);
            _mediaFactory.loadPlugin( pluginResource ); 

        }
        
        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.

            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.

    loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        }
        
        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }
        
        
        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;
            
            state =  event.state;
            
            switch (state)
            {
                case MediaPlayerState.LOADING: 
                    
                    // A new source is started to load.
                    
                    break;
                
                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 
                    
                    break;
                
                case MediaPlayerState.BUFFERING :
                    
                    break;
                
                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }
        
        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }
        
        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.
            
            var resource:URLResource= new URLResource( sourceURL );
            
            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     
        
    }
}

##<a name="strobe-media--playback-with-the-ss-odmf-dynamic-plugin"></a>Stroboskoopin median toisto ja SS ODMF dynaaminen laajennus

Tasainen Streaming OSMF dynaaminen laajennuksen on yhteensopiva [Stroboskoopin median toiston (SMP)](http://osmf.org/strobe_mediaplayback.html). Voit käyttää OSMF laajennuksen SS tasainen Streaming sisällön toistaminen lisääminen SMP. Voit tehdä tämän kopioida "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" verkkopalvelimeen HTTP kuormituksen seuraavalla tavalla:

1.  Selaa [Stroboskoopin median toisto määritys-sivulle](http://osmf.org/dev/2.0gm/setup.html). 
2.  Määritä src tasainen Streaming lähteeseen, (esimerkiksi http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest) 
3.  Uudet määritykset haluamasi muutokset ja valitse Esikatselu ja Päivitä.
 
    **Huomautus** Sisällön verkkopalvelin on kelvollinen crossdomain.xml. 
4.  Kopioi ja liitä koodi yksinkertaisen HTML-sivun käyttämällä tuttuja tekstieditorilla, kuten seuraavassa esimerkissä:



        <html>
        <body>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest &autoPlay=true"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars=" src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true">
        </embed>
        </object>
        </body>
        </html>



5. Tasainen Streaming OSMF-laajennuksen lisääminen upotuskoodi ja Tallenna.

        <html>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10">
        </embed>
        </object>
        </html>


6.  HTML-sivulla Tallenna ja julkaise verkkopalvelimeen. Siirry käyttämällä tuttuja Flash julkaisit&reg; toisto-ohjelman käytössä Internet-selaimen (Internet Explorer, Chrome, Firefox, jne.).
7.  Nauti tasainen mediavirtaa sisällä Adobe&reg; Flash&reg; Player.

Lisätietoja yleistä OSMF kehitystä avata virallinen [OSMF kehittäminen sivun](http://osmf.org/resources.html).

##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="see-also"></a>Katso myös

[Microsoft mukautuvat Streaming laajennuksen OSMF päivityksen](https://azure.microsoft.com/blog/2014/10/27/microsoft-adaptive-streaming-plugin-for-osmf-update/) 
