<properties
 pageTitle="Ennakoivan ylläpito valmiiksi ratkaisu | Microsoft Azure"
 description="Azure IoT valmiiksi ennakoivan ylläpito-ratkaisun kuvaus."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="stevehob"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/17/2016"
 ms.author="araguila"/>

# <a name="predictive-maintenance-preconfigured-solution-overview"></a>Ennakoivan ylläpito valmiiksi ratkaisun yleiskatsaus

*Ennakoivan ylläpito* valmiiksi ratkaisu on yksi [esimääritetyt ratkaisut] [ lnk_preconfigured_solutions] julkaissut osana [Microsoft Azure IoT Suite][lnk_iot_suite]. Tämä ratkaisu integroituu reaaliaikainen laitteen telemetriatietojen sivustokokoelman luotu [Azure koneen Learning]ennakoivan mallin[lnk_machine_learning].


Azure IoT Suite yrityksille voit nopeasti ja helposti muodostaa valvoa varat ja analysoida tietoja reaaliajassa. Ennakoivan valmiiksi ylläpito-ratkaisun on tietoja ja käyttää monipuolisia raporttinäkymien ja visualisointeja antaa yrityksille, jotka vaikuttavat tehokkuus ja parantaa tuotto virtaa uusia Liiketoimintatieto.

## <a name="the-scenario"></a>Skenaario

Fabrikam on alueellisen lentoyhtiön, ohjeessa on hyvä käyttökokemuksen kilpailukyvyn hintaan. Yksi lennot aiheuttaa ylläpito ongelmat ja ilma ohjelma ylläpito on erityisen hankalaa. Ohjelma virheen lennon on voidaan välttää kaikki costs, jotta Fabrikam tarkistaa sen ohjelmien säännöllisesti ja noudattaa suunniteltu ylläpito-ohjelma. Ilma-ohjelmien aina ei käytettävä sama. Jotkin tarpeettomat ylläpito suoritetaan ohjelmien. Ongelmat syntyä, jossa jauhettuina ilma, kunnes ylläpito suoritetaan. Tämä aiheuttaa kallista viiveitä, etenkin jos ilma on tehty, jossa oikea teknisen tai osia ei ole käytettävissä.

Fabrikam's ilma ohjelmien ovat instrumented anturit, jotka ohjelma ehdot valvoa lennon kanssa. Fabrikam Azure IoT Suite avulla voit kerätä lennon tunnistimen tietoja. Kun ohjelma toiminnallisia kertyvät vuosina ja virheen tiedot, Fabrikam henkilön tiedot tutkijoiden on mallintaa voi ennustaa jäljellä olevan hyötyä aika (RUL) ilma-moduulin. Mitä ne tunnistaneet on neljä ohjelma anturit kanssa, joka johtaa potentiaalisen virheen ohjelma käytettävä tiedot korrelaatio. Kun Fabrikam edelleen suorittaa säännöllisesti tarkastukset turvallisuuden varmistamiseksi, se nyt laskea kunkin Engine RUL jokaisen lento avulla telemetriatietojen kerännyt kohteesta ohjelmien lennon mallien avulla. Fabrikam nyt ennustaa tulevia kohtiin virheen ja ylläpidon suunnitteleminen ja korjata etukäteen.

> [AZURE.NOTE] Todellinen ohjelma käytettävä tietoja käyttävän ratkaisu-malli.

Korrelaatio kohdassa, kun ylläpito tarvitaan, mukaan Fabrikam voi optimoida toimintojaan pienentämiseksi. Ylläpito koordinaattorit käsitellä aikatauluttajien suunnitella ylläpito äärimmäisellä ilma pysäyttäminen tiettyyn kohtaan ja varmistaa, että riittävästi aikaa ilma on poissa käytöstä ilman aiheuttaa häiriöitä aikataulu. Fabrikam voit ajoittaa teknisen. varmistamiseksi ilma määrä tehokkaasti ilman odotusaika. Varaston hallinta valvojat saa ylläpito suunnitelmat, jotta niiden järjestäminen prosessin optimointi ja vara osat varaston. Nämä mahdollistaa Fabrikam Pienennä ilma maasta aikaa ja vähentää kustannuksia ja varmistaa, että matkustajien ja miehistön turvallisuus.

Selvittääksesi, miten [Azure IoT Suite] [ lnk_iot_suite] sisältää ominaisuudet-asiakkaiden on huomaat ennakoivan ylläpito mahdollisuuksia, tutustu tämän [infographic][lnk_infographic].

## <a name="how-the-predictive-maintenance-solution-is-built"></a>Miten ennakoivan ylläpito-ratkaisun muodostanut

Ratkaisu hyödyntää käytettävissä mallina näyttämään näiden ominaisuuksien sijainnista laitteen telemetriatietojen IoT Suite services kerättyjen aiempi Azure koneen Learning malli. Microsoft on luonut [regression mallin] [ lnk_regression_model] ilma-moduulin ja julkaista valmis malli, tietojen<sup>\[1\]</sup>, ja mallin käyttämisestä vaiheittaisia ohjeita.

Azure IoT valmiiksi ennakoivan ylläpito-ratkaisun käyttää luotu tukimalli; regression-malli se on otettu käyttöön Azure tilaukseen ja tarjoamia automaattisesti luotu API-Liittymän kautta. Ratkaisu on 4 (/ 100 yhteensä) edustava testauksen tietojen alijoukon ohjelmien ja 4 (21 yhteensä) tunnistimen tietojen virtaa jotka tarjoavat tarkkoja tuloksen, koulutetun mallista.

*\[1\] A. Saxena ja K. Goebel (2008). "Ohivirtausmoottorien ohjelma heikkeneminen simuloinnissa tietojoukon", NASA Ames Prognostics tietojen tietovarasto (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), NASA Ames Research Center Moffett kentän, CA*

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja siitä, miten Azure IoT mahdollistaa ennakoivan ylläpidon tilanteissa, luku [sieppaaminen asioita Internet-arvoa][lnk_capture_value].

Ota [ongelmatilanteita] [ lnk-predictive-walkthrough] ennakoivan huollosta valmiiksi ratkaisu.

[lnk-predictive-walkthrough]: iot-suite-predictive-walkthrough.md
[lnk_preconfigured_solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk_iot_suite]: iot-suite-overview.md
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk_infographic]: https://www.microsoft.com/server-cloud/predictivemaintenance/Index.html
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3
[lnk_capture_value]: http://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF

Voit myös selata joitakin muita ominaisuuksia ja toimintoja IoT Suite valmiiksi ratkaisuja:

- [Usein kysyttyjä kysymyksiä IoT Suite][lnk-faq]
- [IoT suojaus on alusta alkaen][lnk-security-groundup]

[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
