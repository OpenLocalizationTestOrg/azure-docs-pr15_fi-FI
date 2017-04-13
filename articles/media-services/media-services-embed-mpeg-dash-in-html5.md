<properties 
    pageTitle="MPEG-VIIVA mukautuvat videoiden upottaminen HTML5-sovellusta, jonka DASH.js | Microsoft Azure" 
    description="Tässä ohjeaiheessa kerrotaan, miten Upota MPEG-VIIVA mukautuvat Streaming Video DASH.js HTML5-sovellukseen." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="embedding-a-mpeg-dash-adaptive-streaming-video-in-an-html5-application-with-dashjs"></a>MPEG-VIIVA mukautuvat videoiden upottaminen DASH.js HTML5-sovellukseen

##<a name="overview"></a>Yleiskatsaus

MPEG-VIIVA on ISO standardin mukautuvat streaming videon sisältöä, joka tarjoaa käyttäjille, jotka haluavat laadukkaita, mukautuvat videoiden streaming tulosteen merkittäviä etuja. MPEG-viivalla videovirtaa automaattisesti pudottaa alemman määritykseen kun verkossa on ruuhkaa. Tämä pienentää näe "keskeytetty" video, kun Media player Lataa (eli puskurointi) toistaminen seuraava hetkinen viewer todennäköisyys. Kun verkon kuormituksen pienentää, videon toisto-ohjelman puolestaan palaa korkeampi laatu-muodossa. Tämä mahdollisuus mukauttaa tarvittavan kaistanleveyden tuottaa myös nopeampi alkamisaika videon. Joka tarkoittaa, että muutama sekunti voi toistaa nopea lataa alemman laatu-osiossa ja valitse vaiheen ylöspäin korkeampi laatu kerran tarpeeksi sisältöä on ollut ellei.

Dash.js on Avaa lähde MPEG-VIIVA videosoittimen kirjoitettu JavaScript. Sen lähinnä antamaan tehokkaat, Office kaikissa ympäristöissä Playerin, joka voi olla vapaasti uudelleen, jotka edellyttävät videon toisto-sovelluksissa. Se sisältää MPEG-VIIVA toisto kaikissa selaimissa, joka tukee nyt W3C Media lähde laajennukset (MSE), joka on Chrome-, Microsoft Edge- ja IE11 (muissa selaimissa on merkitty niiden palveluita tukemaan MSE). Lisätietoja DASH.js js artikkelissa GitHub dash.js säilö.


##<a name="creating-a-browser-based-streaming-video-player"></a>Selainpohjainen streaming videoiden toisto luominen

Voit luoda yksinkertaisen verkkosivulle, joka näyttää videosoittimen kanssa odotettu ohjaa esimerkiksi toista, tauko, Kelaa jne., sinun on:

1. HTML-sivun luominen
1. Videon tunniste
1. Lisää dash.js player
1. Alusta player
1. CSS-tyylien lisääminen
1. Tarkasteleminen selaimessa, joka toteuttaa MSE

Valmistellaan Playerin voidaan suorittaa vain muutama JavaScript-koodia rivit. Käytä dash.js, todella on, että helppoa MPEG-VIIVA videon selainpohjainen-sovelluksissa.

##<a name="creating-the-html-page"></a>HTML-sivun luominen

Ensimmäiseksi on luotava Vakio **videon** osan Tallenna tämän tiedoston basicPlayer.html, joka sisältää HTML-sivu, kuten seuraavassa esimerkissä havainnollistetaan:

    <!DOCTYPE html>
    <html>
      <head><title>Adaptive Streaming in HTML5</title></head>
      <body>
        <h1>Adaptive Streaming with HTML5</h1>
        <video id="videoplayer" controls></video>
      </body>
    </html>

##<a name="adding-the-dashjs-player"></a>DASH.js toisto-ohjelman lisääminen

Voit lisätä sovelluksen dash.js viittaus käyttöönoton, sinun on käytetty dash.js projektin julkaisuversio 1.0 dash.all.js-tiedostosta. Tämä on tallennettava sovelluksen JavaScript-kansiossa. Tiedosto on helppokäyttöisyys tiedosto, joka hakee yhteen kaikki tarvittavat dash.js koodi yhteen tiedostoon. Jos sinulla on ohjeaiheessa dash.js tietovarasto ympärille, Etsi yksittäisiä tiedostoja, koodi ja paljon muuta testaaminen, mutta kaikki, mitä haluat tehdä on käytössä dash.js ja valitse dash.all.js-tiedosto on tarvittavat.

Voit lisätä sovellustesi dash.js toisto-ohjelman, basicPlayer.html pään osa komentosarjan tunnisteen lisääminen:

    <!-- DASH-AVC/265 reference implementation -->
    < script src="js/dash.all.js"></script>


Seuraavaksi luodaan alustaa Playerin sivun latautuessa funktiota. Lisää rivi, johon olet ladannut dash.all.js jälkeen seuraavaa komentosarjaa:

    <script>
    // setup the video element and attach it to the Dash player
    function setupVideo() {
      var url = "http://wams.edgesuite.net/media/MPTExpressionData02/BigBuckBunny_1080p24_IYUV_2ch.ism/manifest(format=mpd-time-csf)";
      var context = new Dash.di.DashContext();
      var player = new MediaPlayer(context);
                      player.startup();
                      player.attachView(document.querySelector("#videoplayer"));
                      player.attachSource(url);
    }
    </script>

Tämä funktio luo ensin DashContext. Tätä käytetään tiettyjen runtime-ympäristön sovelluksen määrittämisestä. Tekniset näkökulmasta määrittää luokat, jotka riippuvuuden lisäämisen framework tulisi käyttää sovelluksen luodessaan. Useimmissa tapauksissa voit käyttää Dash.di.DashContext.

Seuraavaksi vahvistaa dash.js framework MediaPlayer ensisijainen luokka. Tähän luokkaan on tärkeä, kuten tarvittavia menetelmiä toistaminen ja keskeyttää, hallitsee videon osan yhteyteen ja hallitsee myös tulkinnassa, jossa kuvataan video esitetään Media esityksen kuvaus (MPD)-tiedoston.

MediaPlayer luokan startup()-funktion nimi on varmistaa, että toisto-ohjelman on valmis, videon. Tämä funktio muun muassa varmistaa, että kaikki tarvittavat luokat (määritelty kontekstin mukaan) on ladattu. Kun player on valmis, voit liittää videon osan siihen attachView()-funktiolla. Näin MediaPlayer Lisää videovirtaa elementtiin ja hallita myös toistoa tarpeen mukaan.

Välittää MPD-tiedoston URL-Osoitteen MediaPlayer niin, että se tietoinen odotetaan toistaa videon. Juuri luomasi setupVideo()-funktio on, joka suoritetaan, kun sivu on täysin ladattu. Toimi samoin latautumasta leipäteksti-osan avulla. Muuta oman <body> elementin:

    <body onload="setupVideo()">

Määritä lopuksi koko videon elementin CSS-Tyylisivujen avulla. Mukautuvat streaming-ympäristössä tämä on erityisen tärkeää koska toistamisen videon koon saattavat muuttua, kun toisto sopeutuu muuttuviin Verkkoehdot. Tässä yksinkertainen esittelyssä pakottaa videon elementti on käytettävissä selainikkunassa 80 % lisäämällä pään osa sivulla seuraavat CSS:
    
    <style>
    video {
      width: 80%;
      height: 80%;
    }
    </style>

##<a name="playing-a-video"></a>Videon toistaminen

Videon toistaminen, osoita selaimen basicPlayback.html-tiedostosta ja valitse näkyviin videosoittimen toista.


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Katso myös

[Videosoittimen sovellusten kehittämiseen](media-services-develop-video-players.md)

[GitHub dash.js säilöön](https://github.com/Dash-Industry-Forum/dash.js) 
