<properties
    pageTitle="Automaattisen skaalauksen poistaminen ja sovelluksen palvelun ympäristön | Microsoft Azure"
    description="Automaattisen skaalauksen poistaminen ja sovelluksen Service-ympäristössä"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="byvinyal"
/>

# <a name="autoscaling-and-app-service-environment"></a>Automaattisen skaalauksen poistaminen ja sovelluksen Service-ympäristössä

Azure App palvelun ympäristöissä tukevat *automaattisen skaalauksen poistaminen*. Voit Automaattinen skaalaus yksittäisten työntekijöiden jakavat arvot tai aikataulun perusteella.

![Työntekijän resurssivarantoon Automaattinen skaalaus-asetukset.][intro]

Automaattisen skaalauksen poistaminen optimoi resurssien käytön mukaan automaattisesti ja pienentäminen ja Sovita budjetin tai ladata profiilin App Service-ympäristössä.

## <a name="configure-worker-pool-autoscale"></a>Määritä työntekijä resurssivarantoon Automaattinen skaalaus

Voit käyttää työntekijä-ryhmän **asetukset** -välilehdessä Automaattinen skaalaus-toimintoja.

![Työntekijä-ryhmän asetukset-välilehti.][settings-scale]

Sieltä liittymän pitäisi olla varsin tuttuja, koska se heikkenee saman, näet kun sovellus-palvelusopimus skaalata. 

![Manuaalinen mittakaava-asetuksia.][scale-manual]

Voit myös määrittää Automaattinen skaalaus-profiiliin.

![Automaattinen skaalaus-asetukset.][scale-profile]

Automaattinen skaalaus-profiileista on hyötyä määrittää oman asteikolta. Näin voit määrittää hyvään suorituskykyyn, ilmenee määrittämällä alaraja mittakaava-arvo (1) ja ennakoitavissa spend pää määrittämällä ylärajan (2).

![Profiilin mittakaava-asetuksia.][scale-profile2]

Kun olet määrittänyt profiilia, voit lisätä Automaattinen skaalaus säännöt skaalata ylös tai alas työntekijä altaan profiilin määrittämiä rajojen sisällä esiintymien määrän. Automaattinen skaalaus säännöt perustuvat arvot.

![Skaalaa sääntö.][scale-rule]

 Mikä tahansa työntekijä resurssivarantoon tai edusta arvot avulla voidaan määrittää Automaattinen skaalaus-sääntöjä. Nämä arvot ovat samassa arvot, voit seurata resurssien sivu kaavioihin tai ilmoitusten määrittäminen.

## <a name="autoscale-example"></a>Automaattinen skaalaus-Esimerkki

Automaattinen skaalaus App Service-ympäristön on esitelty parhaiten kävelemällä tilanne kautta.

Tässä artikkelissa kerrotaan kaikki tarvittavat seikat, kun määrität Automaattinen skaalaus. Artikkelissa esitellään vuorovaikutukset, jotka tulevat toista kyselyjä, kun olet huomioon automaattisen skaalauksen poistaminen App Service-ympäristössä, joita isännöidään App Service-ympäristössä.

### <a name="scenario-introduction"></a>Skenaario esittely

Viljo on sysadmin yrityksen, joka on siirretty osa toiminnoista, jotka hän hallitsee sovelluksen Service-ympäristössä.

Sovelluksen Service-ympäristö on määritetty Manuaalinen näytöstä seuraavasti:

* **Päättyy eteen:** 3
* **Työntekijän resurssivarantoon 1**: 10
* **Työntekijän resurssivarantoon 2**: 5
* **Työntekijän resurssivarantoon 3**: 5

Työntekijän resurssivarantoon 1 käytetään tuotannon toiminnoista, vaikka työntekijä resurssivarantoon 2 ja työntekijä resurssivarantoon 3 käytetään laadun varmistaminen (Kysymysosio) ja kehitys toiminnoista.

Sovelluksen palvelusopimusten vaihtoehdot kysymysten ja vastausten ja keskihajonta on määritetty Manuaalinen asteikko. Tuotannon App palvelusopimus on määritetty Automaattinen skaalaus käsitellään liikenteen ja kuormituksen variaatiot.

Viljo on varmasti tutulta-sovelluksella. Hän tietää, että kuormituksen huippu-tuntia on välillä 9:00 ja 6:00 PM liiketoiminta-(LOB)-sovellus, jonka avulla poissaolon Office työntekijät, koska. Käyttö pienenee sen jälkeen, kun käyttäjät ovat päivän. Huippu tuntia ulkopuolella on edelleen joitakin kuormituksen käyttäjät voivat käyttää sovelluksen etäyhteyden avulla niiden mobiililaitteiden tai kotitietokoneessa. Tuotannon App palvelusopimus on jo määritetty Automaattinen skaalaus suorittimen käyttö on seuraavien sääntöjen mukaisesti:

![LOB app tiettyjä asetuksia.][asp-scale]

|   **Automaattinen skaalaus-profiili – viikonpäivät – sovelluksen palvelusopimus**     |   **Automaattinen skaalaus-profiili – viikonloput – sovelluksen palvelusopimus**     |
|   ----------------------------------------------------    |   ----------------------------------------------------    |
|   **Nimi:** Viikonpäivä-profiili                               |   **Nimi:** Viikonloppu-profiili                               |
|   **Skaalata:** Aikataulun ja suorituskyvyn säännöt            |   **Skaalata:** Aikataulun ja suorituskyvyn säännöt            |
|   **Profiili:** Viikonpäivät                                   |   **Profiili:** Viikonlopun                                    |
|   **Tyyppi:** Toistuvat tapahtumat                                    |   **Tyyppi:** Toistuvat tapahtumat                                    |
|   **Kohdealue:** 5-20 esiintymiä                     |   **Kohdealue:** 3-10 esiintymiä                     |
|   **Päivän:** Maanantai, tiistai, keskiviikko, torstai, perjantai  |   **Päivän:** Lauantai, sunnuntai                              |
|   **Alkamisaika:** 9:00 AM                                 |   **Alkamisaika:** 9:00 AM                                 |
|   **Aikavyöhyke:** UTC-08                                   |   **Aikavyöhyke:** UTC-08                                   |
|                                                           |                                                           |
|   **Automaattinen skaalaus säännön (asteikko ylöspäin)**                           |   **Automaattinen skaalaus säännön (asteikko ylöspäin)**                           |
|   **Resurssi:** Tuotannon (sovelluksen palvelun ympäristössä)      |   **Resurssi:** Tuotannon (sovelluksen palvelun ympäristössä)      |
|   **Arvo:** SUORITTIMEN %                                       |   **Arvo:** SUORITTIMEN %                                       |
|   **Toiminto:** Yli 60 %                         |   **Toiminto:** Yli 80 %                         |
|   **Kesto:** 5 minuuttia                                 |   **Kesto:** 10 minuutin                                |
|   **Ajan koostaminen:** Keskiarvo                           |   **Ajan koostaminen:** Keskiarvo                           |
|   **Toiminto:** Suurentaa Laske 2                         |   **Toiminto:** Suurentaa Laske 1                         |
|   **Hienoja alaspäin (minuutteina):** 15                             |   **Hienoja alaspäin (minuutteina):** 20                             |
|                                                           |                                                           |
  	|   **Automaattinen skaalaus säännön (asteikko ALANUOLI)**                     |   **Automaattinen skaalaus säännön (asteikko ALANUOLI)**                         |
|   **Resurssi:** Tuotannon (sovelluksen palvelun ympäristössä)      |   **Resurssi:** Tuotannon (sovelluksen palvelun ympäristössä)      |
|   **Arvo:** SUORITTIMEN %                                       |   **Arvo:** SUORITTIMEN %                                       |
|   **Toiminto:** Pienempi kuin 30 %                            |   **Toiminto:** Pienempi kuin 20 %                            |
|   **Kesto:** 10 minuutin                                |   **Kesto:** 15 minuuttia                                |
|   **Ajan koostaminen:** Keskiarvo                           |   **Ajan koostaminen:** Keskiarvo                           |
|   **Toiminto:** Pienentää Laske 1                         |   **Toiminto:** Pienentää Laske 1                         |
|   **Hienoja alaspäin (minuutteina):** 20                             |   **Hienoja alaspäin (minuutteina):** 10                             |

### <a name="app-service-plan-inflation-rate"></a>Sovelluksen palvelun suunnitelman inflaatiota

Sovelluksen palvelu aikoo, jotka on määritetty Automaattinen skaalaus tee niin enimmäisnopeus tunnissa. Tämä kurssi voidaan laskea Automaattinen skaalaus-säännön annettujen arvojen perusteella.

Tietoja ja laskemisesta *App palvelun suunnitelman inflaatiota* on tärkeää sovelluksen palvelun ympäristön Automaattinen skaalaus, koska työntekijä resurssivarantoon asteikko muutokset eivät ole välittömästi.

Sovelluksen palvelun suunnitelman inflaatiota lasketaan seuraavan kaavan avulla:

![Sovelluksen palvelun suunnitelman inflaatiota laskenta.][ASP-Inflation]

Automaattinen skaalaus – asteikko määrittäminen säännön viikonpäivä profiilin tuotannon App palvelusopimus perusteella:

![Sovelluksen palvelun suunnitelman inflaatiota varten viikonpäivät Automaattinen skaalaus – asteikko määrittäminen säännön perusteella.][Equation1]

Automaattinen skaalaus – asteikko määrittäminen säännön viikonlopun profiilin tuotannon App palvelusopimus kyseessä kaavan ratkaista avulla:

![Sovelluksen palvelun suunnitelman inflaatiota varten viikonloput Automaattinen skaalaus – asteikko määrittäminen säännön perusteella.][Equation2]

Tämän arvon voi myös laskea asteikko-luettelosta toimintoja varten.

Automaattinen skaalaus – asteikko alaspäin säännön viikonpäivä profiilin tuotannon App palvelusopimus perusteella tätä näyttää seuraavasti:

![Sovelluksen palvelun suunnitelman inflaatiota varten viikonpäivät Automaattinen skaalaus – asteikko alaspäin säännön perusteella.][Equation3]

Automaattinen skaalaus – asteikko alaspäin säännön viikonlopun profiilin tuotannon App palvelusopimus kyseessä kaavan ratkaista avulla:  

![Sovelluksen palvelun suunnitelman inflaatiota varten viikonloput Automaattinen skaalaus – asteikko alaspäin säännön perusteella.][Equation4]

Tuotannon App palvelusopimus voi kasvaa enimmäisnopeus kahdeksan esiintymät/tunnin viikon aikana ja neljä esiintymät/tunnin viikonloppuisin. Voit vapauttaa tapauksissa enimmäisnopeus neljä esiintymät/tunnin viikon aikana ja kuusi esiintymät/tunnin viikonloput.

Jos useita sovelluksen palvelusopimusten vaihtoehdot isännöidään työntekijä varannon, sinun on laskettava *yhteensä inflaatiota* laskemalla yhteen kaikkien kierretään App Service-suunnitelmien inflaatiota isännöinti työntekijä kyseiseen resurssivarantoon.

![Useita sovelluksen palvelun suunnitelmien ylläpidettävä työntekijä resurssivarantoon yhteensä inflaatiota laskentaan.][ASP-Total-Inflation]

### <a name="use-the-app-service-plan-inflation-rate-to-define-worker-pool-autoscale-rules"></a>Sovelluksen palvelun suunnitelman inflaatiota avulla voit määrittää työntekijän resurssivarantoon Automaattinen skaalaus säännöt

Työntekijän jakavat isännöivien App palvelu aikoo, jotka on määritetty Automaattinen skaalaus täytyy kohdentaa kapasiteetin puskurin. Puskurin sallii Automaattinen skaalaus-toiminnot, jotka suurennus ja pienennys App palvelusopimus tarpeen mukaan. Puskurin vähimmäiskoko olisi laskettu yhteensä App palvelun suunnitteleminen inflaatiota.

Koska App palvelun ympäristön asteikko toimintojen kestää jonkin aikaa, jos haluat käyttää, muutoksia olisi huomioon tarvittaessa muutoksia, joka voi johtua asteikko-toiminnon ollessa käynnissä. Tämä viive sopimaan Suosittelemme, että käytät esiintymät, jotka on lisätty vähimmäismäärä laskettu yhteensä App palvelun suunnitteleminen inflaatiota kutakin Automaattinen skaalaus-toimintoa.

Nämä ohjeet Viljo voidaan määrittää seuraavat Automaattinen skaalaus-profiilin ja säännöt:

![Automaattinen skaalaus profiilin säännöt LOB esimerkiksi.][Worker-Pool-Scale]

|   **Automaattinen skaalaus-profiili – viikonpäivät**                        |   **Automaattinen skaalaus-profiili – viikonloput**                |
|   ----------------------------------------------------    |   --------------------------------------------    |
|   **Nimi:** Viikonpäivä-profiili                               |   **Nimi:** Viikonloppu-profiili                       |
|   **Skaalata:** Aikataulun ja suorituskyvyn säännöt            |   **Skaalata:** Aikataulun ja suorituskyvyn säännöt    |
|   **Profiili:** Viikonpäivät                                   |   **Profiili:** Viikonlopun                            |
|   **Tyyppi:** Toistuvat tapahtumat                                    |   **Tyyppi:** Toistuvat tapahtumat                            |
|   **Kohdealue:** 13 25 esiintymiä                    |   **Kohdealue:** 6-15 esiintymiä             |
|   **Päivän:** Maanantai, tiistai, keskiviikko, torstai, perjantai  |   **Päivän:** Lauantai, sunnuntai                      |
|   **Alkamisaika:** 7:00 AM                                 |   **Alkamisaika:** 9:00 AM                         |
|   **Aikavyöhyke:** UTC-08                                   |   **Aikavyöhyke:** UTC-08                           |
|                                                           |                                                   |
|   **Automaattinen skaalaus säännön (asteikko ylöspäin)**                           |   **Automaattinen skaalaus säännön (asteikko ylöspäin)**                   |
|   **Resurssi:** Työntekijän resurssivarantoon 1                             |   **Resurssi:** Työntekijän resurssivarantoon 1                     |
|   **Arvo:** WorkersAvailable                            |   **Arvo:** WorkersAvailable                    |
|   **Toiminto:** Pienempi kuin 8                              |   **Toiminto:** Pienempi kuin 3                      |
|   **Kesto:** 20 minuutin ajan                                |   **Kesto:** puolessa tunnissa                        |
|   **Ajan koostaminen:** Keskiarvo                           |   **Ajan koostaminen:** Keskiarvo                   |
|   **Toiminto:** Suurentaa Laske 8                         |   **Toiminto:** Suurentaa Laske 3                 |
|   **Hienoja alaspäin (minuutteina):** 180                            |   **Hienoja alaspäin (minuutteina):** 180                    |
|                                                           |                                                   |
|   **Automaattinen skaalaus säännön (asteikko ALANUOLI)**                         |   **Automaattinen skaalaus säännön (asteikko ALANUOLI)**                 |
|   **Resurssi:** Työntekijän resurssivarantoon 1                             |   **Resurssi:** Työntekijän resurssivarantoon 1                     |
|   **Arvo:** WorkersAvailable                            |   **Arvo:** WorkersAvailable                    |
|   **Toiminto:** Suurempi kuin 8                           |   **Toiminto:** Suurempi kuin 3                   |
|   **Kesto:** 20 minuutin ajan                                |   **Kesto:** 15 minuuttia                        |
|   **Ajan koostaminen:** Keskiarvo                           |   **Ajan koostaminen:** Keskiarvo                   |
|   **Toiminto:** Pienentää Laske 2                         |   **Toiminto:** Pienentää Laske 3                 |
|   **Hienoja alaspäin (minuutteina):** 120                            |   **Hienoja alaspäin (minuutteina):** 120                    |

Profiilissa määritetyn kohdealue lasketaan App palvelusopimus + puskurin profiilissa määritetyn pienin esiintymät.

Suurin arvo olisi suurin alueiden ylläpidettävä työntekijä resurssivarantoon App palvelun Palvelupaketit summa.

Vähintään 1 X-akselille App palvelun suunnittelu-inflaatiota on asetettava sääntöjä mittakaava Suurenna määrän.

Vähenemisen laskeminen voidaan säätää johonkin 1/2 X tai 1 X-akselille App palvelun suunnittelu-inflaatiota välillä alaspäin.

### <a name="autoscale-for-front-end-pool"></a>Automaattinen skaalaus edusta resurssivarantoon varten

Säännöt edusta Automaattinen skaalaus on helpompaa kuin työntekijä jakavat. Pääasiassa kannattaa  
Varmista, että kesto ja cooldown ajastimia harkitse sovelluksen palvelusopimus asteikko toimenpiteet eivät ole välittömästi.

Tässä skenaariossa Viljo tietää virhe nopeus kasvaa, kun etu päät saavuttaa 80 % suorittimen käyttö ja määrittää Automaattinen skaalaus-sääntöä niin, että esiintymät seuraavasti:

![Edustatietokannan resurssivarantoon Automaattinen skaalaus-asetukset.][Front-End-Scale]

|   **Automaattinen skaalaus-profiili – eteen päädyt**              |
|   --------------------------------------------    |
|   **Nimi:** Automaattinen skaalaus – edestä päättyy                |
|   **Skaalata:** Aikataulun ja suorituskyvyn säännöt    |
|   **Profiili:** Everyday                           |
|   **Tyyppi:** Toistuvat tapahtumat                            |
|   **Kohdealue:** 3-10 esiintymiä             |
|   **Päivän:** Everyday                              |
|   **Alkamisaika:** 9:00 AM                         |
|   **Aikavyöhyke:** UTC-08                           |
|                                                   |
|   **Automaattinen skaalaus säännön (asteikko ylöspäin)**                   |
|   **Resurssi:** Edustatietokannan resurssivarantoon                    |
|   **Arvo:** SUORITTIMEN %                               |
|   **Toiminto:** Yli 60 %                 |
|   **Kesto:** 20 minuutin ajan                        |
|   **Ajan koostaminen:** Keskiarvo                   |
|   **Toiminto:** Suurentaa Laske 3                 |
|   **Hienoja alaspäin (minuutteina):** 120                    |
|                                                   |
|   **Automaattinen skaalaus säännön (asteikko ALANUOLI)**                 |
|   **Resurssi:** Työntekijän resurssivarantoon 1                     |
|   **Arvo:** SUORITTIMEN %                               |
|   **Toiminto:** Pienempi kuin 30 %                    |
|   **Kesto:** 20 minuutin ajan                        |
|   **Ajan koostaminen:** Keskiarvo                   |
|   **Toiminto:** Pienentää Laske 3                 |
|   **Hienoja alaspäin (minuutteina):** 120                    |

<!-- IMAGES -->
[intro]: ./media/app-service-environment-auto-scale/introduction.png
[settings-scale]: ./media/app-service-environment-auto-scale/settings-scale.png
[scale-manual]: ./media/app-service-environment-auto-scale/scale-manual.png
[scale-profile]: ./media/app-service-environment-auto-scale/scale-profile.png
[scale-profile2]: ./media/app-service-environment-auto-scale/scale-profile-2.png
[scale-rule]: ./media/app-service-environment-auto-scale/scale-rule.png
[asp-scale]: ./media/app-service-environment-auto-scale/asp-scale.png
[ASP-Inflation]: ./media/app-service-environment-auto-scale/asp-inflation-rate.png
[Equation1]: ./media/app-service-environment-auto-scale/equation1.png
[Equation2]: ./media/app-service-environment-auto-scale/equation2.png
[Equation3]: ./media/app-service-environment-auto-scale/equation3.png
[Equation4]: ./media/app-service-environment-auto-scale/equation4.png
[ASP-Total-Inflation]: ./media/app-service-environment-auto-scale/asp-total-inflation-rate.png
[Worker-Pool-Scale]: ./media/app-service-environment-auto-scale/wp-scale.png
[Front-End-Scale]: ./media/app-service-environment-auto-scale/fe-scale.png
