<properties 
    pageTitle="Streaming lokit ja konsoli" 
    description="Streaming lokit ja konsolin yleiskuvaus" 
    authors="btardif" 
    manager="wpickett" 
    editor="" 
    services="app-service\web" 
    documentationCenter=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/12/2016" 
    ms.author="byvinyal"/>

# <a name="streaming-logs-and-the-console"></a>Streaming lokit ja konsoli

## <a name="streaming-logs"></a>Streaming lokit

**Azure portaalissa** on integroitu streaming log katseluohjelman, jolla voit tarkastella jäljityksen tapahtumia **App Service** -sovelluksista reaaliajassa.  

Tämän ominaisuuden määrittäminen edellyttää muutaman helpon vaiheen:

- Kirjoita jäljittää koodissa
- Ottaa käyttöön sovelluksen sovelluksen **vianmäärityslokit**
- Valmiin **Streaming lokit** käyttöliittymässä stream tarkasteleminen **Azure portal**.

### <a name="how-to-write-traces-in-your-code"></a>Viestin kirjoittaminen jäljittää koodissa ###

Kirjoittaminen jäljittää koodi on helppoa.  C# on yhtä helppoa kuin kirjoittaminen seuraava koodi:

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

Jäljitys-luokan sijaitsevat System.Diagnostics nimitilan.

Node.js-sovelluksessa voit kirjoittaa päästä samaan tulokseen koodi:

`````````````````````````
console.log("My trace statement").
`````````````````````````

### <a name="how-to-enable-and-view-the-streaming-logs"></a>Ottaminen käyttöön ja tarkastella toistamisen lokit
![][BrowseSitesScreenshot]Diagnostiikan otetaan käyttöön kohti app välein. Aloita siirtymällä haluat ottaa tämän ominaisuuden käyttöön sivustoon.  
  
![][DiagnosticsLogs]Asetukset-valikosta Siirry **Seuranta** -osioon ja valitse sitten **(1) vianmäärityslokit**. Valitse **(2) Ota** **Sovelluksen loki käyttöön (tiedostojärjestelmän)** tai **Sovelluksen Logging (blob)** **taso** -vaihtoehdon avulla voit muuttaa jäljittää tallentaaksesi vakavuustaso. Jos yrität vain tutustuminen ominaisuus-arvoksi **yksityiskohtainen** , jotta kaikki Jäljitä komennon kerätään taso.

**Tallenna** sivu yläosassa ja olet valmis, voit tarkastella lokitiedot.

>[AZURE.NOTE] Mitä enemmän resursseja on käytetty log ja lisää jäljittää **vakavuustaso** on valmistettu. Varmista, että **vakavuustaso** on määritetty oikein saa tuotannon tai suuri tietoliikenne sivuston. 

![][StreamingLogsScreenshot]Voit tarkastella **toistamisen lokit** -Azure portaalin valitsemalla myös kohdassa **seurannan** asetukset-valikossa **(1) Log-muodossa** . Jos sovellus on aktiivisesti kirjoittaa jäljitys lauseita, valitse olisi ne näkyvät **(2) streaming kirjautuu Käyttöliittymän** lähelle reaaliajassa.

## <a name="console"></a>Konsolin
**Azure portal** konsoli käyttää sovelluksen avulla. Voit tarkastella sinua sovelluksen tiedostojärjestelmässä ja powershell/cmd komentosarjojen suorittamisen. Samoilla oikeuksilla, jotka on määritelty käynnissä sovelluksen koodin suoritettaessa console-komennot ovat sido. Pääsy suojattu kansioita tai suorittamalla komentosarjoja, jotka edellyttävät järjestelmänvalvojan oikeuksia on estetty.  

![][ConsoleScreenshot]Asetukset-valikosta Selaa alaspäin kohtaan **Kehitystyökalut** -osassa ja napsauta **Console (1)** ja **(2) konsolin** Käyttöliittymän avautuu oikealle.

Tutustuminen **konsoli**, kokeile peruskomennot, kuten:

`````````````````````````
dir
`````````````````````````

`````````````````````````
cd
`````````````````````````

<!-- Images. -->
[DiagnosticsLogs]: ./media/web-sites-streaming-logs-and-console/diagnostic-logs.png
[BrowseSitesScreenshot]: ./media/web-sites-streaming-logs-and-console/browse-sites.png
[StreamingLogsScreenshot]: ./media/web-sites-streaming-logs-and-console/streaming-logs.png
[ConsoleScreenshot]: ./media/web-sites-streaming-logs-and-console/console.png
