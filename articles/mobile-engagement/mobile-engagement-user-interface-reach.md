<properties 
   pageTitle="Azure Mobile välitys käyttöliittymä - Reach" 
   description="Opettele saavuttaminen sovelluksen käyttäjien kanssa käyttämällä Azure Mobile välitys push-ilmoitukset" 
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


# <a name="how-to-reach-out-to-the-users-of-your-application-with-push-notifications"></a>Vastausosoitteen push-ilmoitukset ja sovelluksen käyttäjille

Tässä artikkelissa kuvataan **Mobile välitys** -portaalin **yhteys** -välilehti. **Mobile välitys** -portaalin avulla voit seurata ja hallita mobile-sovellukset. Huomaa, että voit käyttää portaalin sinun on **Azure Mobile välitys** -tilin luominen. Lisätietoja on artikkelissa [Azure Mobile välitys-tilin luominen](mobile-engagement-create.md).

Käyttöliittymän Reach-osassa on Push markkinointikampanja-hallintatyökalun, jossa voit luoda/Muokkaa/Aktivoi/valmis/näyttö ja Hae tilastoja Push ilmoituksen kampanjat ja ominaisuudet, joita voit käyttää myös saavuttaa Ohjelmointirajapinnan (ja osia vähäistä Push API) kautta. Muista, että onko käytössäsi API tai Käyttöliittymän, sinun on integroida Azure Mobile välitys ja kanssa SDK-paketissa, ennen kuin voit käyttää eri ympäristöissä sovellukseen Reach saavuttaa kampanjat.

>[AZURE.NOTE] Monta **Mobile välitys** -portaalin Käyttöliittymän osat sisältävät **Näytä Ohje** -painiketta. Paina tätä painiketta osan tilannekohtaiset lisätietoa.

## <a name="four-types-of-push-notifications"></a>Neljänlaisia Push-ilmoitukset
1.    Ilmoitukset - Salli mainosten lähettämiseen käyttäjille, jotka ohjaaminen toiseen paikkaan sovelluksen sisällä tai lähettää ne verkkosivulle tai kaupan sovelluksen ulkopuolella. 
2.    Kyselyiden - mahdollistavat tietojen kerääminen loppukäyttäjät pääomasijoitusprojekteihin liittyviä kysymyksiä.
3.    Tietoja työntää - avulla voit lähettää binaarinen tai base64-datatiedosto. Voit muokata käyttäjien nykyinen versio sovelluksen sovelluksen lähetetään tietojen push tietoja. Sovelluksen on oltava voivat käsitellä tietoja push tiedot.

## <a name="campaign-details"></a>Markkinointikampanja-tiedot

Voit muokata, Kloonaa, poista tai aktivoida kampanjat, joka ei ole aktivoitu vielä osoittamalla heidän nimensä tai jonka valitsemalla voi avata ne. Voit kopioida kampanjat, jotka on jo aktivoitu osoittamalla heidän nimensä tai jonka valitsemalla voi avata ne. Kuitenkin ei voi muuttaa markkinointikampanjan, kun se on aktivoitu.
 
![Reach1][18]

## <a name="reach-feedback"></a>Saavuttaa palaute

Valitse tietojasi Reach markkinointikampanjan **tilastotiedot** . **Yksinkertainen** näkymän näyttää sarakkeen pylväskaaviossa siitä, mitä on tapahtunut sen jälkeen, kun markkinointikampanjan on aktivoitu muodossa visuaalisessa muodossa. **Lisäasetukset** -näkymä on eritellympiä push-markkinointikampanjan tietoja. Nämä tiedot eivät ole käytettävissä, jos haluat lähettää testi markkinointikampanjan eli push, lähetetty testi laitteeseen. Näin miten olisi tulkita nämä tiedot:

1. **Pushed** - määrittää, miten laitteisiin viestien määrä. Määrä riippuu määrittämäsi luotaessa push-markkinointikampanjan kohdeyleisön. Jos et määritä minkä tahansa kohdeyleisön, sitten tämän push lähetetään rekisteröity laitteisiin. Kaikki muut push palvelut, kuten Microsoft ei push-ilmoitukset suoraan laitteita, mutta sen sijaan push niitä vastaaviin ympäristö tiettyjä Push-ilmoituksen palveluja (PNS - APN/GCM/WNS) niin, että ne voit toimittaa sen laitteet. 

2.  **Toimitettu** - viestit, joihin onnistuneesti toimittaman PNS laitteeseen ja kuitata määrän määrittää kuin vastaanotettu Mobile välitys SDK mukaan. 
        
    *Toimitettu syyt määrä on pienempi kuin kuljettama määrä:*
    
    1. Jos käyttäjä on poistettu laitteesta sovelluksen, mutta PNS ei tunnista se lähetämme painallus PNS milloin viesti poistetaan.
    2. Jos laite on sovelluksen, mutta itse laitteet on offline-tilassa pitkäksi aikaa, PNS epäonnistuu toimittamaan viestin laitteeseen. 
    3. Jos viestin perille laitteeseen, mutta Mobile välitys SDK-sovelluksessa ei tunnista viestin sisältöä, se avautuu viestin. Tämä voi johtua mukautus ilmoitus-sovelluksessa, luo poikkeuksen, jossa on todellisen SDK ja pudota viestin. Tämä saattaa esiintyä myös silloin, kun laitteeseen sovellus käyttää versiota Mobile välitys SDK-paketissa, joka ei pysty ymmärtää platform push viestin uudempi versio, mutta tämä on vain silloin, kun sovellus on päivitetty sen jälkeen, kun ilmoituksessa, joka on lähetetty service-ympäristössä. **Lisäasetukset** -välilehdessä kertoo, kuinka monta viestien poistettiin. 
    4. IOS-laitteissa joskus ei Hae perille joko laitteen ollessa vähäinen tai jos sovellus on muissa power merkittävän määrän käsiteltäessä remote ilmoitukset. Tämä on iOS-laitteita rajoitus.   

3.  **Näytettävät** - määrittää viestit, joihin näkyvät onnistuneesti laitteeseen muodossa järjestelmän push/ulos-ja-sovelluksen ilmoituksen ilmoituksen keskelle tai -sovelluksen ilmoituksella sisällä mobile-sovellus app-käyttäjien määrä.  **Lisäasetukset** -välilehdessä kertoo, kuinka monta on järjestelmäilmoituksia ja kuinka monta on-sovellusten ilmoitukset. 
    
    *Näytettävät syyt määrä on pienempi kuin Toimitettu määrä (odottaa näyttäminen)*
    
    1. Jos ilmoituksen markkinointikampanjan oli tai päättymispäivä on mahdollista ilmoituksen toimitettiin, mutta kun aika oli Avaa ja tuo näkyviin sovelluksen käyttäjälle, se on vanhentunut, jotta se on aina näytössä.   
    2. Jos ilmoituksessa, joka on-sovelluksen ilmoituksella ilmoitus näkyy vain, kun sovellus avautuu sovelluksen käyttäjä. Jossa sovelluksen käyttäjä ei ole avannut sovelluksen tapauksissa SDK ilmoittaa, että ilmoituksen toimitettu mutta ei ole vielä näytetä, ennen kuin sovellus avataan. 
    2. Jos ilmoitus on-sovelluksen ilmoituksella ja määritetty toimitettu näytetään tietyn tehtävän/näytössä, valitse myös ilmoituksen ilmoitetaan, mutta ei ole vielä toimitettu asti käyttäjä avaa sovelluksen tietyn näytössä. 
    
4.  **Käyttäjän vuorovaikutus** - viestit, joihin sovelluksen käyttäjä on käsitellyt määrän määrittää sisältyvät viestit, joihin actioned tai lopetettu. 

    - *Sovelluksen käyttäjä voi toiminnon ilmoituksen jommallakummalla seuraavista tavoista:*
            
        1. Jos ilmoitus on järjestelmän/ulos-ja-sovelluksen ilmoituksella tai sovelluskohtaisesta ilmoitus lähetetään vain ilmoitus-ja valitse sovelluksen käyttäjä napsauttaa ilmoituksen.
        2. Jos ilmoitus on-sovelluksen ilmoituksella tekstiä tai web-näkymässä tai kyselyiden sitten sovelluksen käyttäjä napsauttaa toiminto-painiketta.
        3. Jos ilmoituksessa, joka on-sovelluksen ilmoituksella web-näkymä sovelluksen käyttäjä napsauttaa URL-osoite [Android vain] web-näkymässä
    
    - *Sovelluksen käyttäjän poistua ilmoituksen jommallakummalla seuraavista tavoista:*
    
        1. Valitsemalla ilmoituksen sulkemispainiketta suoraan. 
        2. Poissa-ruudun tai poistamasta ilmoituksen. 
        3. Sovellusten ilmoitukset, joiden teksti-web-sisältöä sekä kyselyiden näkyvät yleensä sovelluksen käyttäjälle kahdessa vaiheessa. Artikkelissa ilmoituksen ensin ja napsauttaessaan sitä ne tarkastella myöhemmin tekstin/web/kyselyn sisältöä. Sovelluksen käyttäjän poistua ilmoituksen joko nämä vaiheet ja Lisäasetukset-näkymän tiedot sieppaa tämä. 

5.  **Actioned** - määrittää viestit, jotka on nimenomaisesti actioned sovelluksen käyttäjän määrä. Tämä on se kertoo, kuinka monta sovelluksen käyttäjille on kiinnostunut viestissä, miten ilmoituksessa kiinnostavat numero. 
 
> [AZURE.NOTE] Valitse iOS- ja Windows ympäristöjen, jos käyttäjällä on sovelluksen Avaa ja markkinointikampanja on "Milloin tahansa"-markkinointikampanjan sitten on mahdollista, että molemmat sovelluksen ja-sovellusten ilmoitukset näkyvät samanaikaisesti. Tämä saattaa aiheuttaa näkyy määrä suurempi kuin toimitettu. Jos käyttäjä on vuorovaikutuksessa tai toiminnot ilmoituksen ja jopa käyttäjän vuorovaikutukset/Actioned määrä voi olla suurempi kuin toimitettu. 


![Reach2][19]

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
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md
 
