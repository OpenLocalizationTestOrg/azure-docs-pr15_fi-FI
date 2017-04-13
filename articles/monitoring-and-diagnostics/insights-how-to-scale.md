<properties
    pageTitle="Skaalaa esiintymän Laske manuaalisesti tai automaattisesti | Microsoft Azure"
    description="Opettele skaalata palvelujen Azure."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2015"
    ms.author="robb"/>

# <a name="scale-instance-count-manually-or-automatically"></a>Skaalaa esiintymän Laske manuaalisesti tai automaattisesti

[Azure-portaalissa](https://portal.azure.com/)voit määrittää manuaalisesti palvelun esiintymän määrää, tai voit parametreja ja määrittää sen automaattisesti tarvittaessa perusteella asteikko. Tämä on yleensä kutsutaan *Mittakaava,* tai *käytetään*.

Ennen kuin esiintymän Laske skaalaus perusteella, ota huomioon, että skaalaus vaikuttaa **hinnoittelu taso** lisäksi esiintymän määrä. Eri hinnoittelu tasojen voi olla erilaisia lukuja Sydämiä ja muistin ja siten heillä on sama määrä esiintymät (joka on *asteikko ylös* tai *alas asteikko*) tehostaviksi. Tässä artikkelissa kerrotaan tarkemmin *käytetään* ja *ulos*.

Voit skaalata portaalissa, ja voit myös käyttää [REST API](https://msdn.microsoft.com/library/azure/dn931953.aspx) tai [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) Säädä asteikko manuaalisesti tai automaattisesti.

> [AZURE.NOTE] Tässä artikkelissa kuvataan, miten voit luoda Automaattinen skaalaus-asetus-portaalissa osoitteessa [http://portal.azure.com](http://portal.azure.com). Luotu portaalin Automaattinen skaalaus-asetuksia ei voi muokata perinteinen portaalin ([http://manage.windowsazure.com](http://manage.windowsazure.com)).

## <a name="scaling-manually"></a>Skaalaus manuaalisesti

1. [Azure-portaali](https://portal.azure.com/)valitsemalla **Selaa**ja valitse Siirry resurssi haluat skaalata, kuten **sovelluksen palvelusopimus**.

2. **Toimintojen** **Skaalaa** -ruutu selviää, skaalaus tila: **käytöstä** kun sinulla on skaalauksen manuaalisesti **,** kun vähintään yksi suorituskyvyn mittarit skaalaamalla.
    ![Skaalaa-ruutu](./media/insights-how-to-scale/Insights_UsageLens.png)

3. -Ruutu valitsemalla vievät sinut **asteikko** -sivu. Asteikko-sivu yläreunassa näet Automaattinen skaalaus-toiminnot historiatiedot palvelu.  
    ![Skaalaa-sivu](./media/insights-how-to-scale/Insights_ScaleBladeDayZero.png)

>[AZURE.NOTE] Tässä kaaviossa näkyy vain toimintoja, jotka tehdään Automaattinen skaalaus. Jos asetat esiintymän Laske manuaalisesti, muutokset eivät näy tässä kaaviossa.

4. Luvun **esiintymät** liukusäätimen avulla voit muuttaa manuaalisesti.
5. Valitse **Tallenna** -komentoa ja sinun olla sovitettu esiintymien määrän lähes välittömästi.

## <a name="scaling-based-on-a-pre-set-metric"></a>Valmiiksi määritettyjen metrijärjestelmä skaalaus perusteella

Jos haluat säätää kerrat mittarin perusteella, valitse haluamasi lisätiedot **asteikon mukaan** -valikko. Esimerkiksi **sovelluksen palvelusopimus** voit skaalata **Suorittimen**prosenttiarvolla.

1. Kun valitset mittarin saat liukusäätimen ja/tai -tekstikehykset esiintymien välillä skaalattava määrä:

    ![Suorittimen prosenteilla asteikko-sivu](./media/insights-how-to-scale/Insights_ScaleBladeCPU.png)

    Automaattinen skaalaus koskaan siirtää palvelun, joka määrittää, riippumatta siitä, että latautuvat rajat ylä- tai alapuolella.

2. Se voit valita lisätiedot kohdealue. Esimerkiksi jos valitsit **suorittimen prosentteina**, voit määrittää kohde keskimääräinen suorittimen kaikki esiintymät koko-palvelussa. Mittakaava, tapahtuu, kun keskimääräisen suorittimen ylittää määrität, vastaavasti-asteikon tapahtuu aina, kun keskimääräisen suorittimen pudottaa valitsemien.

3. Napsauta **Tallenna** -komentoa. Automaattinen skaalaus tarkistaa muutaman minuutin välein, varmista, että olet esiintymän alueen ja kohdesivustojen oman metrijärjestelmän. Kun palvelun vastaanottaa muita, näkyviin tulee esiintymiä ilman mitään.

## <a name="scale-based-on-other-metrics"></a>Muut arvot perusteella asteikko

Voit skaalata kuin Esiasetukset, **skaalata** avattavassa valikossa näkyvät myös monimutkaisia käytettävissä olevia mittakaava ja skaalata sääntöjen mukaan arvot.

### <a name="adding-or-changing-a-rule"></a>Lisäämään tai muuttamaan säännön

1. Valitse **Aikataulu ja suorituskyvyn säännöt** **asteikon mukaan** -valikko: ![suorituskyvyn säännöt](./media/insights-how-to-scale/Insights_PerformanceRules.png)

2. Jos haluat käyttää aiemmin Automaattinen skaalaus, näet-näkymän tarkka säännöt, jotka haluat käyttää.

3. Skaalata perusteella toisen metrisillä valitsemalla **Lisää sääntö** -rivi. Voit myös napsauttaa aiemmin riveillä käyttöoikeutensa takaisin haluat skaalata metrijärjestelmä metrijärjestelmä muuttaminen.
![Säännön lisääminen](./media/insights-how-to-scale/Insights_AddRule.png)

4. Nyt sinun on valittava mitä metrijärjestelmä haluat skaalata. Kun valitset mittarin on muutama huomioon otettavat asiat:
    * *Resurssin* lisätiedot tulee. Tavallisesti tämä on sama kuin resurssi on skaalaus. Jos haluat skaalata tallennustilan jonon syvyys, resurssi on jonossa, jonka haluat skaalata.
    * *Metrijärjestelmän nimi* itse.
    * *Ajan koostaminen* lisätiedot. Tämä tieto siitä, miten tiedot on yhdistäminen *kesto*.

5. Kun olet valinnut oman metrijärjestelmä Valitse lisätiedot ja operaattori raja-arvo. Voit esimerkiksi sanoa **yli** **80 %**.

6. Valitse toiminto, jonka haluat ottaa. Liittyy muutama erityyppisen toiminnot:
    * Suurentaa tai pienentää – tämä Lisää tai poista **arvo** kerrat, voit määrittää
    * Suurenna tai Pienennä prosentti – Tämä muuttaa esiintymän määrä prosenttia. Esimerkiksi voi lisätä 25 **arvo** -kenttään, ja jos olisit 8 esiintymät, 2 lisättäisiin.
    * Suurentaa tai pienentää – tämä asetus esiintymän Laske Määritä **arvo** .

7. Voit valita lopuksi jäähdytetään alaspäin - kuinka kauan säännöllä odottaa skaalata uudelleen edellisen asteikko-toiminnon jälkeen.

8. Säännön määrittämisen jälkeen valitse **OK**.

9. Kun olet määrittänyt kaikki haluamasi säännöt, muista osumien **Tallenna** -komentoa.

### <a name="scaling-with-multiple-steps"></a>Useita vaiheita skaalaus

Yllä esimerkkejä on melko perustiedot. Jos haluat lisää kohdetoimialueen tietoja skaalaus ylös (tai alas), voit myös lisätä useita saman metrijärjestelmän asteikko sääntöjä. Voit esimerkiksi määrittää kahden asteikko sääntöjä suorittimen prosentteina:

1. Skaalata ulos 1 esiintymän Jos suorittimen prosenttiosuus on yli 60 %
2. Skaalata 3 esiintymät Jos suorittimen prosentti on suurempi kuin 85 %

![Useita asteikko sääntöjä](./media/insights-how-to-scale/Insights_MultipleScaleRules.png)

Muita säännöllä kanssa Jos että latautuvat ylittää 85 % ennen asteikko-toimintoa, näyttöön tulee kaksi uusia esiintymiä yhden sijaan.

## <a name="scale-based-on-a-schedule"></a>Aikataulun mukaan asteikko


Oletusarvoisesti, kun luot asteikko säännön, se on aina käyttää. Näet, kun napsautat profiilin tunnistetietojen:

![Profiili](./media/insights-how-to-scale/Insights_Profile.png)

Haluat ehkä kuitenkin on enemmän kohdetoimialueen skaalaus päivän tai viikon aikana kuin viikonloppuisin. Voi myös Sammuta palvelun kokonaan käytöstä työtunnit.

1. Voit tehdä tämän profiilin on, valitse **Toistuminen** sijaan **aina,** ja valitse ajankohdat, jolloin haluat käyttää profiilia.

2. Esimerkiksi antaaksesi profiilia, joka koskee viikon aikana **päivän** avattavan valikon Poista **lauantai** ja **sunnuntai**.

3. Profiilia, joka koskee aikana daytime-asetukseksi on **Aloitusaika** kellonaika, jolle haluat alkaa.
    ![Oletusarvoinen Toistuminen](./media/insights-how-to-scale/Insights_ProfileRecurrence.png)

4. Valitse **OK**.

5. Seuraavaksi tarvitset lisää profiili, jota haluat käyttää eri aikoina. Valitse **Lisää profiili** -rivi.
    ![Ei töissä](./media/insights-how-to-scale/Insights_ProfileOffWork.png)

6. Uusi toisen profiilin nimi, esimerkiksi voi soittaa se **ei töissä**.

7. Valitse Valitse **Toistuminen** uudelleen ja valitse haluamasi esiintymän Laske alueen tänä aikana.

8. Oletusprofiilin, valita **päivää** haluat profiilia, jos haluat käyttää, ja **Aloitusaika** päivän aikana.

>[AZURE.NOTE] Automaattinen skaalaus käyttää kesä säästöjen sääntöjä kumpi **aikavyöhykkeen** valitset. Kuitenkin UTC-poikkeama näkyy perus aikavyöhyke-siirtymä ei kesä säästöjen UTC-poikkeama kesäajan aikana.

9. Valitse **OK**.

10. Nyt tarvitset lisää jostakin sääntöjä, johon haluat lisätä toisen profiilin aikana. Valitse **Lisää sääntö**ja sitten aikana oletusprofiili on sama sääntö voi käyttää.
    ![Lisää sääntö käytöstä työmäärä](./media/insights-how-to-scale/Insights_RuleOffWork.png)

11. Varmista, että molemmat säännön mittakaava ja skaalaus- or else aikana profiilin esiintymän määrä vain kasvaa (tai Pienennä).

12. Valitse lopuksi **Tallenna**.

## <a name="next-steps"></a>Seuraavat vaiheet

* [Näytön palvelun mittarit](insights-how-to-customize-monitoring.md) , varmista, että palvelu on käytettävissä ja vastaa.
* Kerätä yksityiskohtaisia hyvin usein arvot palvelun [käyttöön seuranta- ja diagnostiikka](insights-how-to-use-diagnostics.md) .
* [Vastaanota ilmoitukset](insights-receive-alert-notifications.md) aina, kun toiminnallisia tapahtuma tapahtuu tai arvot toimintojen raja-arvon.
* Voit halutessasi selvittääksesi, miten pilveen osassa koodisi toimii [näytön sovelluksen suorituskykyä](../application-insights/app-insights-azure-web-apps.md) .
* [Tapahtumien tarkasteleminen ja valvontalokit](insights-debugging-with-events.md) saat kaikki, joka on tapahtunut palvelussa.
* [Näytön käytettävyys- ja minkä tahansa verkkosivun vasteaikaa](../application-insights/app-insights-monitor-web-app-availability.md) sovelluksen tiedot, jotta voit tarkistaa, jos sivu on poissa käytöstä.
