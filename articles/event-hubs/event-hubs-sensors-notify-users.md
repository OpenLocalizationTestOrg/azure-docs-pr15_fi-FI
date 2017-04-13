<properties 
   pageTitle="Ilmoita käyttäjille anturit tai muista järjestelmistä saadut tiedot | Microsoft Azure"
   description="Tässä artikkelissa käsitellään Ilmoita käyttäjille tunnistimen tietojen tapahtuman keskittimet avulla."
   services="event-hubs"
   documentationCenter="na"
   authors="spyrossak"
   manager="timlt"
   editor="" />
<tags 
   ms.service="event-hubs"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="spyros;sethm" />

# <a name="notify-users-of-data-received-from-sensors-or-other-systems"></a>Ilmoita käyttäjille anturit tai muista järjestelmistä saadut tiedot

Oletetaan, että sinulla on sovellus, joka valvoo tietojen reaaliajassa tai tuottaa raportteja aikataulun. Jos tarkastelet, kyseisten reaaliaikainen kaavioita tai raportit näkyvät-sivustosta, saatat nähdä jotakin, mitä edellyttää. Entä jos sinun on oltava Hälytetyn tilanteisiin sijaan, voit tarkistaa sivuston muistaminen varmenteen käyttäjän? Kuvitellaan, että sinulla kasvihuonekaasujen Suurenna valo ja sinun tarvitsee tietää heti, jos valo siirtyy. Yksi tapa tehdä olisi kanssa Vaalea tunnistimen kasvihuone, valitse järjestäminen voidaan lähettää sähköpostia, jos valo on poistettu käytöstä.

![][1]

Toisen tilanteessa Kuvitellaan, että suoritat lemmikkieläinten lennolle tilojen ja sinun on saada ilmoituksia pienen varaston toimituksen tasot. Voit järjestää esimerkiksi voidaan lähettää tekstiviestin ERP-järjestelmästä, jos oman varastoluettelo koiran elintarvikkeiden laskenut kriittinen tasolle. 

![][2]

Ongelma on hankkiminen tärkeitä tietoja, kun tietyt ehdot täyttyvät, ei, kun haluat staattisen raportin uloskuittaamiseen siirrytään. Jos käytät [Azure tapahtumaa-toiminnossa][] tai [Azure IoT keskittimeen][] tietojen vastaanottamiseksi laitteet-tai enterprise-sovellusten, kuten [Dynamics AX][], sinun on useita vaihtoehtoja, voit käsitellä niitä. Voit tarkastella niitä sivusto, voit analysoida, voit tallentaa ne ja niiden avulla voidaan käynnistää komennot tekemään tiettyjä toimenpiteitä. Voit tehdä tämän käyttämällä tehokkaita työkaluja, kuten [Azure-sivustoja][], [SQL Azure][], [HDInsight][], [Cortana liiketoimintatietojen Suite][], [IoT Suite][], [Logiikan sovellukset][]tai [Azure ilmoituksen keskittimet][]. Mutta joskus kaikki haluamasi toimet on, että tietojen lähettäminen jonkun katseltavan vähintään. Jos haluat näyttää, kuinka voit tehdä vain hieman koodin kanssa, on määritetty uuden otoksen [AppToNotifyUsers][]. Asetuksia ovat sähköposti (SMTP), SMS ja puhelinnumero.

## <a name="application-structure"></a>Sovelluksen rakenne

Sovellus on kirjoitettu C# ja otoksessa Lueminut-tiedosto sisältää kaikki tiedot, joita haluat muokata, luoda ja julkaista sovellus. Seuraavissa osissa on yleiskatsaus sovelluksen mitä.

Aluksi olettaen, että niiden ollessa Azure tapahtumaa-toiminnossa tai IoT keskittimeen tärkeitä tapahtumia. Mikä tahansa keskittimeen tällöin toimi, kunhan pääsevät käyttämään sitä ja tiedät yhteysmerkkijono.

Jos sinulla ei ole tapahtumaa-toiminnossa tai IoT keskittimeen, voit helposti määrittää testi kukkapenkistä Arduino shield ja vadelma pii [Yhteyden pistettä](https://github.com/Azure/connectthedots) projektin ohjeiden mukaisesti. Valitse Arduino shield valaistustunnistin lähettää pii – Vaalea tasot [Azure tapahtumaa-toiminnossa][] (**ehdevices**) ja [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) -työ Vie ilmoitusten toisen tapahtumaa-toiminnossa (**ehalerts**), jos vastaanotettu Vaalea tasoa alemmaksi tietyn tason.

**AppToNotify** käynnistyessä lukee kokoonpanotiedosto (App.config) URL-Osoitteen ja tunnistetiedot hakeminen ilmoitusten vastaanottaminen tapahtumaa-toiminnossa. Se sitten etäasemassa prosessin seurannassa jatkuvasti, että tapahtumaa-toiminnossa viesti, jossa on kautta –, kunhan sinulla on pääsy tapahtumaa-toiminnossa tai IoT keskittimeen ja kelvolliset tunnistetiedot URL-osoite, tapahtuman keskittimet reader koodi lukee jatkuvasti löytyy tilaukseesi. Käynnistyksen aikana sovellus myös lukee valitut URL-Osoitteen ja tunnistetiedot, jota haluat käyttää tekstiviesti-palvelun (sähköposti, Tekstiviesti-puhelin) ja nimi ja osoite lähettäjän ja vastaanottajien luetteloa.

Kun tapahtumaa-toiminnossa näytön havaitsee viestin, se tuottaa prosessi, joka lähettää viestin määritettyyn määritystiedostossa menetelmällä. Huomaa, että se lähettää se havaitsee viesteissä. Jos määrität näyttöä osoittavan tapahtuma-keskittimeen, joka vastaanottaa kymmenen viestit sekunnissa, lähettäjälle lähetetään kymmenen viestien sekunnissa – kymmenen sähköpostit sekunnissa 10 tekstiviestien sekunnissa kymmenen puhelut sekunnissa. Tästä syystä Varmista, että valvoa tapahtumaa-toiminnossa, joka vastaanottaa ilmoituksia, jotka on lähetetty, ei-tapahtumaa-toiminnossa, joka vastaanottaa raaka tietojen anturit ja sovelluksia, vain.

## <a name="applicability"></a>Puolivälissä

Tässä esimerkissä koodi näkyy vain, miten voit valvoa tapahtuman keskittimet ja kutsua ulkoisia palveluihin silloin, kun haluat lisätä sovelluksen toiminto. Huomaa, että tämä ratkaisu DIY developer keskittyvässä Esimerkki vain. Se ei käsitellä yrityksen vaatimukset, kuten redundanssi korvaava-uudelleen mukaan vika jne. Lisätietoja kattava ja tuotannon kysymyksestä on seuraavissa artikkeleissa:

- Käyttämällä yhdistimet tai push-ilmoitukset [Azure logiikan sovellukset](../app-service-logic/app-service-logic-connectors-list.md) -palvelun avulla.
- Käyttäminen [Azure ilmoituksen keskittimet](https://msdn.microsoft.com/library/azure/jj927170.aspx)blogin [lähetyksen push-ilmoitukset ja mobiililaitteiden avulla Azure ilmoituksen keskittimet miljoonia](http://weblogs.asp.net/scottgu/broadcast-push-notifications-to-millions-of-mobile-devices-using-windows-azure-notification-hubs)kuvatulla tavalla. 

## <a name="next-steps"></a>Seuraavat vaiheet

On helppoa, voit luoda yksinkertaisen ilmoituspalvelu, joka lähettää sähköpostiviestejä tai tekstiviestejä vastaanottajille tai soittaa ne välitys tiedot vastaanottanut tapahtumaa-toiminnossa tai IoT toiminnossa. Ilmoita käyttäjille tietojen vastaanottanut nämä keskittimet perustuvan ratkaisun ottamaan käy [AppToNotifyUsers][].

Lisätietoja näiden keskittimet seuraavissa artikkeleissa:

- [Azure tapahtuman keskittimet]
- [Azure IoT-toiminnossa]
- Aloittaminen [tapahtuman keskittimet opetusohjelma].
- Valmis [tapahtuman keskittimet käyttävän sovelluksen malli].

Voit ottaa ratkaisun Ilmoita käyttäjille vastaanottanut nämä keskittimet tietojen perusteella, seuraavasta:

- [AppToNotifyUsers][]

[Tapahtuman keskittimet opetusohjelma]: event-hubs-csharp-ephcs-getstarted.md
[Azure IoT-toiminnossa]: https://azure.microsoft.com/services/iot-hub/
[Azure tapahtuman keskittimet]: https://azure.microsoft.com/services/event-hubs/
[Azure tapahtumaa-toiminnossa]: https://azure.microsoft.com/services/event-hubs/
[Tapahtuman keskittimet käyttävä malli-sovellus]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[AppToNotifyUsers]: https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications
[Dynamics AX]: http://www.microsoft.com/dynamics/erp-ax-overview.aspx
[Azure sivustot]: https://azure.microsoft.com/services/app-service/web/
[SQL Azure-tietokannassa]: https://azure.microsoft.com/services/sql-database/
[Hdinsightiin]: https://azure.microsoft.com/services/hdinsight/
[Cortana liiketoimintatietojen Suite]: http://www.microsoft.com/server-cloud/cortana-analytics-suite/Overview.aspx?WT.srch=1&WT.mc_ID=SEM_lLFwOJm3&bknode=BlueKai
[IoT Suite]: https://azure.microsoft.com/solutions/iot-suite/
[Logiikan sovellukset]: https://azure.microsoft.com/services/app-service/logic/
[Azure ilmoituksen keskittimet]: https://azure.microsoft.com/services/notification-hubs/
[Azure Stream Analytics]: https://azure.microsoft.com/services/stream-analytics/
 
[1]: ./media/event-hubs-sensors-notify-users/event-hubs-sensor-alert.png
[2]: ./media/event-hubs-sensors-notify-users/event-hubs-erp-alert.png