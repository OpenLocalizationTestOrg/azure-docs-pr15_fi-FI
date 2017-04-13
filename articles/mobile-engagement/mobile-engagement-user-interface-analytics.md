<properties
   pageTitle="Azure Mobile välitys käyttöliittymä - Analytics"
   description="Lue, miten voit analysoida historiatietoja sovelluksesi käyttämällä Azure Mobile välitys"
   services="mobile-engagement"
   documentationCenter=""
   authors="piyushjo"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile"
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="how-to-analyze-historical-data-about-your-application"></a>Voit analysoida historiatietoja sovelluksen

Tässä artikkelissa kuvataan **Mobile välitys** -portaalin **ANALYTICS** -välilehti. **Mobile välitys** -portaalin avulla voit seurata ja hallita mobile-sovellukset. Huomaa, että voit käyttää portaalin sinun on **Azure Mobile välitys** -tilin luominen.


Käyttöliittymän Analytics-osassa on koottuja tietoja sovelluksesta historiatietoihin, joka päivittyy 24 tunnin välein perusteella. Tiedot näkyvät eri raporttinäkymät koostuva rivin/palkin/Ympyräkaaviot, ruudukko ja kartat. Voit ladata tiedot myös .csv-tiedostoina. Useimmat tämän samat tiedot on käytettävissä reaaliajassa käyttöliittymän näyttö-kohta ja se voi käyttää myös Analytics-Ohjelmointirajapinnan.

>[AZURE.NOTE] Monta **Mobile välitys** -portaalin Käyttöliittymän osat sisältävät **Näytä Ohje** -painiketta. Paina tätä painiketta osan tilannekohtaiset lisätietoa.

## <a name="standard-and-custom-analytics"></a>Vakio- ja mukautettuja Analytics

Azure Mobile välitys on basic, Vakio, joka on graphed heti, kun sovellus integroida SDK sovellusten analyyttisten tiedot. Azure Mobile välitys mahdollistaa myös kerää muita mukautettuja analytics-tiedot, jotka haluat tietoja oman loppukäyttäjien toiminnasta. Voit tehdä tämän luomalla mukautettuja "tunnisteita (sovelluksen tiedot)"-luotu **asetukset** niin, että Azure Mobile välitys voi kerätä tämän tiedot puolestasi tunnisteen suunnitelma.



## <a name="analytics"></a>Analytics
- Koontinäyttö: Näyttää yleisiä tietoja uusien ja actives käyttäjien ja niiden trendejä.
- Käyttäjät: Käyttäjät tunnistetaan niiden laitteen tunnus: tunnus on yksilöllinen kunkin laitteen (yksi uusi käyttäjä on todella yksi uusi laite). Käyttäjän pidetään uutena tietyn aikavälin, jos hän on suorittanut hänen ensimmäisen istunnon aikana tähän aikaväli. Käyttäjän pidetään säily, jos hän on suorittanut vähintään yksi istunto viimeisen seitsemän päivän aikana. Aktiiviset käyttäjät ovat käyttäjät, jotka on tehty vähintään yksi istunto tietyn ajan kuluessa. Voit lajitella, viikoittain, kuukausittain päivittäin tai kerran tunnissa ajanjaksojen. Kaikkien kaavioiden muistuttaa mutta salli suodattamalla eri ominaisuuksia, kuten sovelluksen-versio ja sitten Lajittele tietyn ajan. Vakio integroimalla SDK kerättyjen tietojen sisältää seuraavat: aktiiviset käyttäjät, uusi käyttäjä, istuntojen pituuden kunkin istuntoon, teknisiä tietoja maa, paikalliset muuttujat, sijainti, kielen carrier, laitteita, laitteisto, verkon (WIFI), sovelluksen ja SDK-versiot käyttää asiakkaille. Nämä tiedot voit tarkastella näytön osasta reaaliajassa.

> Huomautus: Aika-asteikon perustuu käyttäjien laiteasetusten päivämäärän jotta käyttäjä, jonka puhelin on määritetty virheellisesti päivämäärä voi enää näy väärä ajan kuluessa.

- Säilytys: Käyttäjä katsotaan kuin säily tietyn aikavälin, jos hän on suorittanut hänen ensimmäisen istunnon aikana tähän aikaväli. Voit muuttaa aikana säilytetä käyttäjät (ja uudet käyttäjät) lasketaan aikavälien tuntia, päivän, viikon tai kuukauden. Käyttäjän säilytys analytics muodostanut cohorts päälle. Cohort on uusia käyttäjiä havaita tietyn ajan (eli suorittamiseen niiden ensimmäisen istunnon tänä aikana käyttäjät määritetty). Käytämme cohorts 1 päivä, päivän 2, 4 päivän, 7 päivän tai 1 kuukausi. Valita cohort, 1 – päivittäin, 2-päivän 4 päivän, 7 päivän tai 1 kuukausi, Azure Mobile välitys laskee kaikki käyttäjät, jotka kuuluvat cohort ja ovat joukko yhä aktiivinen (eli käyttäjät, jotka suoritetaan ainakin yhden istunnon aikana määritetty). Tämä käyttäjäryhmä kutsutaan cohort-versio. (Azure Mobile välitys voi näyttää, kuinka monta käyttäjät yhä käytössä sovelluksen, mutta vain ympäristö kaupan tietää, kuinka monta käyttäjät poistanut app – esimerkiksi GooglePlay iTunes-Windows-kaupan jne.).
- Istunnot: Yksi käyttäjä sovelluksen käyttö. Istunnot luodaan toiminnot, jotka suoritetaan käyttäjien järjestyksen (aktiviteetin liittyy yleensä yksi näyttö-sovelluksen käyttö, mutta tämä vaihtelevat sen mukaan, miten SDK on integroitu sovelluksessa). Käyttäjä voi suorittaa yhdestä aktiviteetin vain kerrallaan: istunnon käynnistyy heti, kun käyttäjä käynnistyy ensimmäisen toimintaansa ja pysähtyy, kun hän on valmis viimeisen toimintaansa. Jos käyttäjä pysyy yli muutaman sekunnin suorittamatta toimintaa, toimintojen hänen järjestyksen jaetaan kaksi eri istuntoa.
- Tehtävät: Kunkin näytön sovelluksen nimet ja pituuden, jonka aika-käyttäjät käyttävät kunkin näytön. Toiminnot ovat mukautettu analyyttisten-vaihtoehto, joka määrittää oman sovelluksen "sovelluksen tiedot-tunnisteet vastaavat:
- Käyttäjän polku: Näyttää, miten käyttäjät siirtyä sovelluksen toiminnot (näyttöä). Voit siirtää liukusäädintä säätää tiedot. Sininen solmujen edustavat sovelluksen toimintoja. Niiden koko on aika käyttäjien käytetyt ei. Valkoinen solmujen edustavat istunnon aloittamisesta ja lopettamisesta. Punainen solmujen edustavat kaatuu. Linkkejä esittävät siirtymät sovelluksen toiminnot (tai toiminnot ja kaatuu välillä). Napsauta solmu tai linkin työkaluvihjeen, jossa haluat lisätietoja tietojen näyttämiseen: tietyn näytön käytetty aika siirtymien määrä ja prosentti siirtymien lähde tehtävästä kohde-toimintoon. (---60 %---> B tarkoittaa, että käyttäjät tehtävän A siirtyy tehtävän B 60 prosenttia aikaa.) Voit järjestää kaavio, kun haluat selventää; sijaintinsa tallennetaan aina, kun teet muutoksia. Voit näyttää tai piilottaa kaavion Vaalenna kaatuu.
- Tapahtumat: Tietyn sovelluksen käyttäjän toimet. Tapahtumien jakautumisen näkyy tapahtumien istunnon käyttäjäkohtainen määränä. Tapahtuman edustaa pikaviestien toimintoa, esimerkiksi painikkeen tai ilmoituksen vastaanotto napsauttamalla. (Tapahtumien merkitys riippuu siitä, miten SDK on integroitu sovelluksen.) Tapahtuman voi ilmetä istunnon tai projektin aikana, tai se voi olla erillinen.
- Töiden: Samalla tapahtumat paitsi ne keskittyä toiminnon pituuden. Töiden voi esimerkiksi kertoa teknisiä tietoja siitä, kuinka kauan rekisteröijän sisällön kuormituksen tai verkkopalvelun kutsu. Kuinka kauan rekisteröijän lomakkeen täyttämisen, Luo tili tai hankintaan käyttäjä voi myös näyttää. Työn vastaa tehtävän keston, esimerkiksi Lataa tehtävän tai aika, kesto nauhan näkyy näytössä. (Työt merkitys riippuu siitä, miten SDK on integroitu sovelluksen.) Töiden liittyy yleensä (eli ilman mitään käyttäjän tehtävä) istunnon laajuus ulkopuolella suoritettavat taustatehtävät.
- Technicals: Teknisiä tietoja laitteiden käyttäjien sovelluksesi seurata, kuten kielen, Carrier, verkon, laite, laitteisto- ja näytön koko käyttäjien laitteiden ja sovelluksesi ja käyttää sovelluksen SDK-versio.
- Virheet: Tekninen sovelluksen sisällä virheitä, jotka eivät aiheuta sovelluksen kaatumaan tietoja. Virhe tarkoittaa pikaviestien ongelma, esimerkiksi verkkovirhe tai virheelliset käytöstä. (Tapahtumien merkitys riippuu siitä, miten SDK on integroitu sovelluksen.) Virhe voi ilmetä istunnon tai projektin aikana, tai se voi olla erillinen.
- Kaatuu: Tietoja virheistä, jotka aiheuttavat sovelluksesi voi kaatua. Kaatumisen on odottamattomia ehto, jossa sovellus lopettaa suorittaessaan odotettu ja on lopetettava. Kaatumisen on yleensä ohjelmavirhe-sovelluksessa.

![Analytics2][11]

## <a name="accessing-the-retention-overview"></a>Säilytys-yleiskatsaus käyttäminen
![Analytics3][12]

Säilytys-yleiskatsaus on eritelty kyselyjä useita kortteja, keskellä kunkin näkyy yhteenveto tiettyjä säilytysaika. Esimerkissä näkyy 2 päivän säilytysaika. Muita kortteja Näytä 4 päivän ja 7 päivän säilytys jaksot.

## <a name="understanding-the-retention-overview-cards"></a>Tietoja säilytys yleiskatsaus-kortit
![Analytics4][13]

### <a name="each-card-is-composed-of-3-main-parts"></a>Kussakin kortissa koostuu 3 pääosaa:
1. 1: cohort ja kauden
2. 2-4: tilikauden säilytys
3. 5: historia sparkline-kaaviossa

### <a name="here-is-detailed-information-about-each-element"></a>Näin jokaisen osan yksityiskohtaisia tietoja:
1.    Cohort ja kauden: Tämä otsikko antaa cohort tyyppi. Tässä "2 päivän" tarkoittaa, että olemme tarkastella käyttäjät toimintaa kaksi päivää, käyttäjät, joilla on saapunut ajanjakson 2 päivää ja onko yhdistämien kaksi päivää seuraavat lohkot. Yllä olevassa esimerkissä katsoo tehtävän 21st ja 22 marraskuun välillä.
2.    Näin säilytys nopeus 21 ja 22 marraskuun saapuvat 19 ja 20 marraskuun käyttäjille. Tähän on oli 1 aktiivisen käyttäjän 21st – 22, 3, jotka olivat uusille käyttäjille 19: nneksi päiväksi – 20 päälle.
3.    Tämän visuaalisen ilmaisimen antaa samat tiedot kuin yllä graafisesti. (Ympyrän kolmannen on 33 % numerosta). Värin antaa seuraavat tiedot: vihreä ilmaisee määrä kasvaa edellisen laskennasta. Keltainen tarkoittaa vakaata ja punainen tarkoittaa pienentäminen.
4.    Tämä ilmaisee laskennassa käytettävät arvot.
5.    Tämä on sparkline-kaavion säilytys arvot historian. Voit nähdä arvot on ja miten kehittynyt laajat näkymän menneisyydessä.


## <a name="see-also"></a>Katso myös

- [Käsitteitä][Link 6]
- [Oppaan palvelun vianmääritys][Link 24]

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md
