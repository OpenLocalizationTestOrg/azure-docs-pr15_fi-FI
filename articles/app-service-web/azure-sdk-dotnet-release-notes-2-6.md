<properties 
   pageTitle="Azure SDK .NET 2.6 julkaisutiedot" 
   description="Azure SDK .NET 2.6 julkaisutiedot" 
   services="app-service/web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>

 
# <a name="azure-sdk-for-net-26-release-notes"></a>Azure SDK .NET 2.6 julkaisutiedot

Tämä asiakirja sisältää julkaisutiedot Azure SDK .NET 2.6-versiossa. 

Azure SDK 2.6 voit kehittää cloud-Palvelusovellusten (PaaS) kohdistamisen .NET 4.5.2 tai .NET 4.6, jos kohde .NET Framework asentaminen Cloud palvelun rooli manuaalisesti. Kohdassa [asentaminen .NET Cloud palvelun rooliin](http://go.microsoft.com/fwlink/?LinkID=309796).


##<a name="service-bus-updates"></a>Palvelun Bus päivitykset

- Tapahtuman keskittimet: 

    - Mahdollistaa nyt kohdennettujen käyttöoikeuksien hallinta, kun muita publisher päätepiste paljastaa tapahtuman keskittimien lähettämisestä tapahtumat.
    - Muita vakauteen ja parantaminen lisätään tapahtuma keskittimet ominaisuus.
    - Lisääminen tuki Amqp-protokollan kautta WebSocket viestintään ja tapahtuman keskittimet.

##<a name="hdinsight-tools-for-visual-studio-updates"></a>HDInsight Tools for Visual Studio päivitykset

- **IntelliSense laajentamisesta**: remote metatietojen ehdotus

    HDInsight Tools for Visual Studio tukee nyt hakeminen remote metatietojen rakenne-komentosarjan muokattaessa. Kirjoita esimerkiksi * *Valitse* FROM** ja taulukoiden nimien näytetään. Myös sarakkeiden nimet näkyvät määritettyäsi taulukon.

- **HDInsight emulaattorin tuki**

    Nyt HDInsight Tools for Visual Studio tue yhdistämistä HDInsight emulaattorin, niin rakenne-komentosarjojen voitu kehittäminen paikallisesti ilman esittely kustannukset, suorita HDInsight-klustereiden vastaan komentosarjoja. 

    Saat lisätietoja viitata [oppaassa](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).

- **Yleinen Hadoop klustereiden tuki HDInsight Tools for Visual Studio** (Ennakkoversio)

    HDInsight Tools for Visual Studio tukevat yleinen Hadoop klustereiden, niin voit käyttää HDInsight Tools for Visual Studio seuraavasti:

    - muodostaa yhteyttä klusterin 
    - Kirjoita rakenne-kysely, jossa on tehostettu IntelliSense/automaattinen-täydentäminen tuki 
    - Näytä kaikki työt yhteyttä klusterin intuitiivisen käyttöliittymän. 

    Saat lisätietoja viitata [oppaassa](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).

##<a name="in-role-cache-updates"></a>Rooli-välimuistin päivitykset

- **Rooli-välimuistin** on päivitetty käyttämään **Microsoft Azure-tallennustilan SDK** versio 4.3. Toistaiseksi **-Roolin välimuisti** käytti Azure tallennustilan SDK version 1.7.

    Asiakkaiden käyttämällä Azure SDK 2,5 tai alla pitäisi päivittää Azure SDK 2.6 ja siirry Azure-tallennustilan SDK uuden version. 

    Tällä hetkellä Azuren tallennustilaan versio 2011-08-18 ajoitetaan voi poistaa 1 elokuun 2016. Kaikki siirrot rooli-välimuistin Azure SDK 2,5 tai alapuolelle, 2.6 on suoritettava tässä vaiheessa. Katso lisätietoja Azure-tallennustilan versio 2011-08-18 käytöstä poistaminen [Microsoft Azure tallennustilan palvelun versio poisto Update: tunniste 2016](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx).

>[AZURE.IMPORTANT]Emme ole julkaisu Azure hallitun välimuisti-palvelun ja Azure-roolin välimuistin 30 marraskuun 2016-käytöstä poistaminen. On suositeltavaa siirtää Azure Redis välimuistin, valmistelussa tämän käytöstä poistaminen. Katso lisätietoja päivämäärät ja siirron ohjeet [joka Azure välimuistin tarjoaa sopii minulle parhaiten?](../redis-cache/cache-faq.md#which-azure-cache-offering-is-right-for-me)

##<a name="azure-app-service-tools"></a>Azure App palvelun Työkalut

Seuraavat kohteet on päivitetty Azure SDK 2.6-versiossa.

- Parannettu sisältämään Azure API sovellusten käyttöönoton kohteeksi Azure julkaiseminen.
- API sovellusten valmistelu toimintoja, jotta käyttäjät API-sovelluksen luominen ja valmisteleminen toimintojen kanssa.
- Palvelimen Explorer muuttaa vastaamaan uusi sovellus palvelun solmu Web Mobile ja API sovelluksissa, jotka on ryhmitelty resurssiryhmä.
- Lisää Azure API App asiakkaan liikkeen useimmissa C#-projekteissa, jotka mahdollistavat Swagger käyttävä API sovellusten käynnissä käyttäjän Azure tilauksen automaattisen luonnin lisätään.
- API sovellusten sillä ja sovelluksen palvelun solmujen palvelimen Explorerissa ovat käytettävissä Visual Studio 2013: n vain. 

##<a name="azure-resource-manager-tools-updates"></a>Azure Resurssienhallinta Työkalut-päivitykset

Azure resurssien hallinnan työkalut on päivitetty sisältämään mallit näennäiskoneiden, verkko ja tallennustilaa. JSON muokkaaminen kokemus on päivitetty sisältämään uusi jäsennysnäkymä-mallit ja malleja käyttämällä JSON katkelmat muokkaus. Mallit käyttöön Visual Studiossa avulla mukana projektiin, jotta komentosarja tehtyjä muutoksia käytetään Visual Studio PowerShell-komentosarjaa.

##<a name="diagnostics-improvements-for-cloud-services"></a>Pilvipalveluihin diagnostiikka parannukset

Azure SDK 2.6 näyttää takaisin Azure Laske emulaattori diagnostiikka lokien kerääminen ja siirtäminen kehittäminen tallennustilan tuki. Mikä tahansa diagnostiikka kirjautuu (mukaan lukien sovelluksen trace lokit-tapahtuman jäljitys (tapahtumien seuranta) Windows-lokit, suorituskyvyn laskureita, infrastruktuuri lokit ja Windowsin tapahtumalokien) luotu kun sovellus on käynnissä emulaattori voidaan siirtää kehittäminen tallennustilan ja tarkista, että Diagnostiikan kirjaus toimii paikallisessa tietokoneessa. 

Diagnostiikka-tallennustilan tilin nyt voidaan määrittää eri ympäristöissä eri diagnostiikka tallennustilan tilien käyttäminen on helpompaa määritys (.cscfg) tiedostossa. On joitakin keskeisiä eroja välillä miten yhteysmerkkijonon työskennellyt Azure SDK 2.4 ja Azure SDK 2.6. Lisätietoja käyttämisestä diagnostiikka tallennustilan yhteyden merkkijono ja miten se vaikuttaa projektien artikkelissa [Azure pilvipalveluihin määrittäminen diagnostiikkaa](http://go.microsoft.com/fwlink/?LinkID=532784).

##<a name="breaking-changes"></a>Tärkeimmät muutokset

###<a name="azure-resource-manager-tools"></a>Azure Resurssienhallinta-Työkalut 

- Käytettävissä olevat Azure SDK 2,5 **Cloud käyttöönottoprojektien** projektityypin on nimetty **Azure resurssiryhmä**.
- **Cloud käyttöönottoprojektien** tyypin Azure SDK 2,5 luotuja projekteja voi käyttää 2.6 mutta käyttöönotto Visual Studiossa mallia epäonnistuu. Kuitenkin käyttöönotto ja PowerShell-komentosarjaa edelleen toimii aiemmin samalta kuin.  Lisätietoja siitä, miten voit käyttää 2.6 **Cloud käyttöönottoprojektien** lukea tämän [viestin](http://go.microsoft.com/fwlink/?LinkID=534086).
 
##<a name="known-issues"></a>Tunnetut ongelmat

- 64-bittinen käyttöjärjestelmä emulaattori diagnostiikka lokien kerääminen edellyttää. Diagnostiikan lokit ei kerätä, kun käyttöjärjestelmä 32-bittinen käyttöjärjestelmä. Tämä ei vaikuta emulaattorin-toimintoja. 

- Azure SDK 2.6 julkaistiin 4/29/2015 oli kaksi ongelmat: 

    - Yleinen sovellus ei voitu ladata Visual Studio 2015: ssä, kun Azure SDK 2.6 on asennettu tietokoneeseen.
    - Virheenkorjaus pilvipalvelussa projektin epäonnistuu Visual Studio 2013 ja Visual Studio 2015, jossa Visual Studio lopettaa vastaamisen ja kaatuu ja näyttää viestin "emulaattorin diagnostiikkaa määrittäminen-valintaikkuna.
    
    Azure SDK 2.6 päivitys julkaistiin 5/18/2015. Päivitetty versio on 2.6.30508.1601; se sisältää kaksi ongelmien kuvattuun korjaukset. Voit tunnistaa Ohjauspaneelista SDK muodosta -> ohjelmat ja toiminnot -> Microsoft Visual Studio 2013 – v 2.6 Microsoft Azure-työkalut. Versio-sarakkeessa näkyy koontiversio.

    Jos ovat edelleen vastakkaisten yllä mainitut ongelmat, asenna [ja 2012](http://go.microsoft.com/fwlink/p/?linkid=323511&clcid=0x409), [ja 2013: n](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) tai [ja 2015](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)Azure 2.6 SDK uusimman version.
 
##<a name="see-also"></a>Katso myös

[Tuki- ja käytöstä poistaminen lisätietoja Azure SDK .NET ja ohjelmointirajapinnan](https://msdn.microsoft.com/library/azure/dn479282.aspx/)
