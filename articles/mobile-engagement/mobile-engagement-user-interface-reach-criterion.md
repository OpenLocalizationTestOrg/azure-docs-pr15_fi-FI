<properties 
   pageTitle="Azure Mobile välitys käyttöliittymä - Reach vertailuarvo" 
   description="Opettele käyttämään kohdentamisesta ehdot push Kampanjat lähettämiseksi Valitse osajoukko käyttämällä Azure Mobile välitys käyttäjät" 
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


# <a name="how-to-use-targeting-criteria-to-send-push-campaigns-to-a-select-subset-of-your-users"></a>Kohdentamisesta ehtojen käyttämisestä push Kampanjat lähettämiseksi Valitse osajoukko käyttäjät

Yleisö Raportointirakenteen mukaan tietyt ehdot "Uusia ehtoja"-painike on tehokas käsitteitä Azure Mobile välitys-että avulla voit lähettää asiaa push-ilmoitukset, että asiakkaiden reagoi sijaan roskapostien kaikille. Voit rajoittaa yleisö vakio ehtojen perusteella ja simuloida työntää voit selvittää, kuinka monta henkilöt saavat ilmoituksen.

**Katso myös:**

- [Käyttöliittymän asiakirjat - Reach - uusi Push markkinointikampanja][Link 27]

## <a name="audience-criteria-can-include"></a>Käyttäjäryhmän ehdot voivat sisältää:
- **Technicals:** Voit kohdistaa samaan teknistä tietoa, näet analyysin ja näytön osien perusteella. **Katso myös:** [Käyttöliittymän asiakirjat - Analytics] [ Link 15]- [Käyttöliittymän asiakirjat - näyttö][Link 16]
- **Sijainti:** Sovellukset, jotka käyttävät "reaaliaikainen sijainti raportointi" Geo aitaus kanssa voit käyttää Geo sijainti ehdot kohdentamisen käyttäjäryhmän GPS sijainnista. "Kuitata alueen sijainti raportointi" puhelun käytetään myös kohdentamisen käyttäjäryhmän Matkapuhelin-sijainnista ("reaaliaikainen sijainti raportointi" ja "Kuitata alueen sijainti raportointi" on aktivoitava SDK: n). **Katso myös:** [Ohjeet SDK - iOS - integrointi] [ Link 5], [SDK asiakirjat - Android - integrointi][Link 5]
- **Saavuttaa palautetta:** Voit kohdistaa yleisö antamaan palautetta edellisen reach ilmoitukset ilmoitukset-kyselyiden ja tietojen Vie Palautetoiminnon reach perusteella. Näin voit parantaa kohteeseen yleisö jälkeen kaksi tai kolme saavuttaa Kampanjat kuin ensimmäisen kerran. Se voidaan myös käyttäjät, jotka ovat jo vastaanottanut ilmoituksen samalla sisällöllä määrittämällä markkinointikampanjan ei lähettämisen käyttäjät, jotka ovat jo vastaanottanut tietyn edellisen markkinointikampanjan suodattamaan. Voit myös jättää käyttäjät käyttävät sisällyttää tiettyä markkinointikampanjan, joka on yhä käytössä vastaanottamisen uusi työntää. **Katso myös:** [Käyttöliittymän asiakirjat - saavuttaa - Push-sisältö][Link 29]
- **Asentaa seuranta:** Voit seurata tietoja, johon käyttäjien asennettu sovelluksen perusteella. **Katso myös:** [Käyttöliittymän asiakirjat - asetukset][Link 20]
- **Käyttäjäprofiilin:** Voit kohdistaa mukaan standard käyttäjätiedot ja voit kohde, jonka olet luonut mukautetun sovelluksen tiedot perusteella. Tämä sisältää käyttäjät, jotka ovat tällä hetkellä kirjautuneena ja käyttäjille, että ne sovelluksessa sen sijaan, että vain siitä, miten ne on vastannut aiempia Määritä ohjelma sinulla on kysymyksiin. Kaikki sovelluksen tiedot määritetty yhteyttä sovelluksen näkyvät tässä luettelossa.
- Lohkot: Voit myös kohde osia, jotka olet luonut perusteella perusteella tietyn käyttäjän ongelman, joka sisältää useita ehtoja. Kaikki oman määritetty sovelluksen osia näy tässä luettelossa. **Katso myös:** [Käyttöliittymän asiakirjat - osia][Link 18]
- **Sovelluksen tiedot:** Mukautetun sovelluksen tiedot tunnisteet voidaan luoda "Asetukset" jäljittämiseksi käyttäjän toiminnan. **Katso myös:** [Käyttöliittymän asiakirjat - asetukset][Link 20]

## <a name="example"></a>Esimerkki: 
Jos haluat siirtää ilmoituksen vain käyttäjät, jotka olet suorittanut-sovellusten hankkiminen toiminnon aliraportti määrittäminen.

1. Siirry sovelluksen asetukset-sivun, valitse "sovelluksen tiedot-valikko ja valitse"Uusi sovellus-tiedot"
2. Rekisteröi uusi totuusarvo sovelluksen tiedot, kutsutaan "inAppPurchase"
3. Tehdä sovelluksesta määritetty tämän sovelluksen tiedot "true", kun käyttäjä suorittaa onnistui-sovellusten hankkiminen (käyttämällä sendAppInfo ("inAppPurchase",...) funktio)
4. Jos et halua tehdä sovelluksen, voit tehdä oman taustasta käyttämällä laitteen API)
5. Sitten juuri on luotava ilmoituksessa, ehdon rajoittaminen yleisö käyttäjät, joilla on "inAppPurchase" arvoksi "true")
 
> Huomautus: Kohdistamisen kuin sovelluksen tiedot tunnisteet ehtojen perusteella edellyttää Azure Mobile varauksia tietojen kerääminen ja käyttäjien laitteita, ennen kuin painallus lähetetään ja siten voi aiheuttaa viiveen. Asetukset (kuten päivittäminen nimilaput) voivat myös viivästyttää monimutkaisia push-määritys vie. Push-Ohjelmointirajapinnan "yhden vahvistetaan" järjestelmän ovat Azure Mobile välitys suora nopein push-menetelmää. Seuraava nopein tapa on vain sovelluksen tiedot-ohjauskoodien käyttäminen ehtoina push Reach markkinointikampanjan (joko saavuttaa Ohjelmointirajapinnan tai Käyttöliittymän) varten, koska palvelinpuolen tallennetaan sovelluksen tiedot tunnisteet. Push-markkinointikampanjan kohdentamisesta ehtojen käyttäminen on eniten joustavia mutta mahdollisimman pieneksi push Azure Mobile välitys on kysely voidaksesi lähettää markkinointikampanja laitteet.
 
![Saatavilla Criterion1][29] 

## <a name="criterion-options-apply-to"></a>Ehdon asetukset koskevat:
- **Technicals**     
- Laitteisto-nimi: laitteisto nimi
- Laitteisto-versio: laitteisto-versio
- Laitteen mallista: laitteen mallista
- Laitteen valmistajan: laitteen valmistajan
- Sovellusversio: sovellusversio
- Carrier nimi: Carrier nimi ei määritetty
- Carrier maa: Carrier maa-Määrittämätön
- Verkon tyyppi: verkon tyyppi
- Aluekohtaiset asetukset: Aluekohtaiset asetukset
- Näytön koko: näytön koko
- **Sijainti**      
- Viimeisin tunnettu alue: maa-alueen sijainti
- Reaaliaikainen geo-aitaus: luettelon, POIs (nimi, toiminnot), pyöreä o (nimen, leveyttä, pituutta, säde on metriä)
- **Saavuttaa palaute**     
- Ilmoitus palautetta: ilmoitus ja palaute
- Äänestyksen järjestäminen palautetta: kysely-palaute
- Kyselyn vastaus palautetta: kyselyn vastaus palautetta, kysymys, valinta
- Tietoja Push palautetta: tietojen Push-palaute
- **Asenna seuranta**     
- Säilö: Tallenna, ei määritetty
- Lähde: Lähde ei määritetty
- **Käyttäjäprofiilin**     
- Sukupuolen: Mies ja nainen, Määrittämätön
- SYNTYMÄAIKA päivämäärä: operaattori, päivämäärän Määrittämätön
- Sisältyy: TOSI tai EPÄTOSI, Määrittämätön
- **Sovelluksen tiedot**      
- Merkkijono: Merkkijono, joka on määrittämätön
- : Operaattori, päivämäärän Määrittämätön
- Kokonaisluku: operaattori, luku, ei määritetty
- Totuusarvo: TOSI tai EPÄTOSI, Määrittämätön
- **Osion**    
- Nimi (avattava luettelo)-luettelosta osia pois jätettyjen (kohde käyttäjät, jotka eivät kuulu tämän osion).

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
 
