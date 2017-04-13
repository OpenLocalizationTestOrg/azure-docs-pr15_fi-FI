<properties
   pageTitle="Tietoja noutaminen Power BI Azure Tietoturvakeskus tietojasi | Microsoft Azure"
   description="Sisällön Azure Security Center Power BI-pakettiin on helppo etsiä suojausvaroitusten, suositukset, hyökkäyksen kohteeksi resurssit ja trendit, tietojoukko, joka on luotu vain omaa raportoinnin perusteella."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="yurid"/>

# <a name="get-insights-from-azure-security-center-data-with-power-bi"></a>Hae tiedot Power BI Azure Tietoturvakeskus tiedoista
[Power BI-koontinäytön](http://aka.ms/azure-security-center-power-bi) Azure Tietoturvakeskus avulla voit havainnollistaa, analysointia ja suodattamista suosituksia ja suojausvaroitukset missä tahansa, myös mobiililaitteeseen. Power BI-koontinäytön avulla voit nähdä trendejä ja hyökkäyksille kuviot - näkymän suojausvaroitusten resurssi tai lähde-IP-osoite ja unaddressed suojausriskejä, resurssin tai ikä. 

Voit voi myös luomisesta Tietoturvakeskus suosituksia ja muiden tietojen kanssa suojausvaroitusten kiinnostavat tavoin käyttäen esimerkiksi [Azure valvontalokien](https://powerbi.microsoft.com/blog/monitor-azure-audit-logs-with-power-bi/) ja [Azure SQL-tietokannan tarkistaminen](https://powerbi.microsoft.com/blog/monitor-your-azure-sql-database-auditing-activity-with-power-bi/)tietoja. Molemmat tarjoavat Power BI-koontinäytöt ja voit viedä tiedot myös Excelin cloud resurssien suojauksen tilan helposti raportointia varten.

##<a name="using-azure-security-center-dashboard-to-access-power-bi"></a>Käyttää Power BI Azure Tietoturvakeskus Raporttinäkymät-ikkunan avulla
Voit myös käyttää Power BI-raportteja Azure Tietoturvakeskus Raporttinäkymät-ikkunan. Noudattamalla tämän tehtävän suorittamiseen: 

1. Napsauta **Azure Tietoturvakeskus** Raporttinäkymät-ikkunan **Power BI: n Selaa** -painiketta.

    ![Yhteyden muodostaminen käyttämällä Power BI Azure Tietoturvakeskus](./media/security-center-powerbi/security-center-powerbi-fig1-new10.png) 

2. **Tutustu Power BI** -sivu avautuu oikeassa reunassa seuraavassa kuvassa esitetyllä tavalla:

    ![Yhteyden muodostaminen käyttämällä Power BI Azure Tietoturvakeskus](./media/security-center-powerbi/security-center-powerbi-fig1-new2.png)

3. Jos olet luomassa Power BI-koontinäytön ensimmäistä kertaa, voit valita jonkin seuraavista vaihtoehdoista **Selaa Power BI** -sivu: 

    - **Suojauksen tietoja Raporttinäkymät-ikkunan**: Valitse tämä vaihtoehto, jos haluat luoda Raporttinäkymät-ikkunan, joka sisältää suojauksen tilan ja viestiketjuissa siirtyminen tunnistuksia. Tämä asetus on enemmän yhteinen DevOps roolin, joka on vastuussa analysoiminen suojaus tilansa ja havaita ilmoitukset-tilauksissa.
    - **Käytännön hallinnan Raporttinäkymää**: Valitse tämä vaihtoehto, jos haluat tarkastella hallinta ja käyttäminen.  Tämä asetus on enemmän yhteinen keskitetyn IT, joka lisää kohdistettu hallinnon varten. He voivat käyttää tätä Raporttinäkymät-ikkunan ja myönnä näkyvyyttä ja tiedot, valitse Suojaus-käytäntö ehdokasvaltio koko organisaatiossa.
    - Jos sinulla on jo Power BI-koontinäytön, valitse **Siirry nykyisen Power BI-koontinäytön**.

4. Valitse tässä esimerkissä **suojauksen havainnollistamisen dashboard** -vaihtoehto. Jos kyseessä on ensimmäisen kerran, sinua kehotetaan Asenna sisällön Tietoturvakeskus Power BI-Raporttinäkymät-ikkunan luodaan. Valitse **sisällön paketteja Power BI** -ikkunassa **Hae** painike seuraavassa kuvassa esitetyllä tavalla:

    ![Azure Security Center suojauksen tietoja Raporttinäkymät-ikkunan](./media/security-center-powerbi/security-center-powerbi-fig1-new3.png)

5. **Yhdistä Azure Security Center suojauksen havainnollistamisen** -ikkunassa näkyvät. Varmista, että **todennusmenetelmä** on **oAuth2** alla kuvatulla tavalla ja **Kirjaudu sisään** -painiketta.
    
    ![Todennus](./media/security-center-powerbi/security-center-powerbi-fig1-new4.png)

6. Sinulta pyydetään todentamismenetelmä Azure tunnistetiedot uudelleen. Raporttinäkymän tarkistamisen jälkeen luodaan. Kun koontinäyttö luodaan näet raportti, jossa on samanlainen rakenne seuraavassa kuvassa esitetyllä tavalla:

    ![Power BI-koontinäyttö](./media/security-center-powerbi/security-center-powerbi-fig1-new5.png)


> [AZURE.NOTE] Raportin päivityksen on varattu suoritetaan päivittäin. Siltä varalta, että tämä päivityksen virhe ilmenee, lue lisätietoja vianmääritys [Mahdolliset Päivitä ongelmat Azure Security Center Power BI](https://blogs.msdn.microsoft.com/azuresecurity/2016/04/07/azure-security-center-power-bi-refresh-fails/).

Tässä näet suojausvaroitusten ja suositukset määrä- ja VMs Azure SQL-tietokannat ja Azure Tietoturvakeskus seurataan verkkoresursseja määrän.

Linkki Azure Tietoturvakeskus ohjaa Azure-portaaliin. Kaavioiden avulla on helppo Visualisoi tietoja suojausta koskevia suosituksia ja ilmoitukset, mukaan lukien:

- Resurssin tietoturvan tilaan
- Odottaa suositukset
- AM suositukset
- Ilmoitusten ajan mittaan
- Hyökkäyksen kohteeksi joutuneen resurssit
- Hyökkäyksen kohteeksi joutuneen IP-osoitteet

Kunkin kaavion takana on muita tietoja. Valitse Saat lisätietoja. Esimerkiksi **Resurssin suojauksen tila** -ruutu näyttää lisätiedot tietoja odottavat suosituksia resurssien mukaan seuraavassa kuvassa esitetyllä tavalla:

![Suosituksia](./media/security-center-powerbi/security-center-powerbi-fig1-new6.png)

Jos valitset tämän kaavion minkä tahansa rivin, muihin sarakkeisiin sujuvat vain valitsemasi-harmaa, ja voit kohdistus. Palaa raporttinäkymät-sivun vasemmanpuoleisessa ruudussa **raporttinäkymät** -vaihtoehto kohdasta **Azure Tietoturvakeskuksessa** .

> [AZURE.NOTE] Jos haluat raporttien mukauttaminen kenttiä lisäämällä tai muuttamalla aiemmin visuaalisia tehosteita, voit muokata raporttia. Lue lisätietoja [kielikoodeista muokkausnäkymän Power BI-taulukkoraporttia käyttäessäsi](https://powerbi.microsoft.com/documentation/powerbi-service-interact-with-a-report-in-editing-view/) .

**Ilmoitusten jaettuina hyökkäyksen kohteeksi resurssit** ja **Hänen IP -osoitteet** ruudut on samanlainen tulosteen yksitellen sitä napsautettaessa. Tämä johtuu siitä, että raportti kokoaa yhteen kaikki nämä kolme muuttujaa koskevat tiedot ja soittaa sen **resurssien hyökkäyksen** seuraavassa kuvassa esitetyllä tavalla:

![Resurssien hyökkäyksen](./media/security-center-powerbi/security-center-powerbi-fig1-new7.png)

Tässä vaiheessa voit myös tallentaa kopion raportissa, tulostaa sen tai julkaista verkossa käyttämällä **Tiedosto** -valikon vaihtoehdot.

![Tiedosto-valikko](./media/security-center-powerbi/security-center-powerbi-fig8.png)

## <a name="exploring-your-azure-security-center-data-with-power-bi-services"></a>Power BI-palvelujen kanssa Azure Tietoturvakeskus tietojen tarkasteleminen

Yhteyden muodostaminen Power BI: n [Power BI sisällön Pack Services](https://msit.powerbi.com/groups/me/getdata/services) ja suorita seuraavat toimet:

1. **Sisällön Pack Power BI** -ikkuna tulee näkyviin kaksi asetusta alla kuvatulla tavalla.

    ![Sisällön pack Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new.png)

    >[AZURE.NOTE] Jos jo suoritettu ensimmäinen osa on tämän artikkelin näet vain yksi vaihtoehto, joka on Azure suojauksen Center käytännön hallinta.

2. Tässä esimerkissä varten Valitse **Azure suojauksen Center käytännön hallinta** -ruudun **Hae** .

3. **Yhteyden muodostaminen Azure suojauksen Center käytännön hallinta** -ikkunassa Varmista, että **oAuth2** alla **Todentamismenetelmän** avattavan kuten-kohdassa ja valitse **Kirjaudu sisään** -painiketta.

    ![Käytännön hallinta-ikkuna](./media/security-center-powerbi/security-center-powerbi-fig1-new8.png)

4. Sinut ohjataan todennusta-sivu, jossa sinun on kirjoitettava tunnistetiedot, jotka käyttävät muodostaa Azure Tietoturvakeskuksessa. Kun todennusprosessin on valmis, Power BI alkavat tuomista raporttien luomiseen. Tänä aikana tulla seuraava sanoma selaimen oikeassa yläkulmassa:

    ![Yhteyden muodostaminen käyttämällä Power BI Azure Tietoturvakeskus](./media/security-center-powerbi/security-center-powerbi-fig4.png)

    >[AZURE.NOTE] Kun koontinäyttö luodaan ensimmäisen kerran saattaa kestää kauemmin kuin yleensä pääasiassa tilanteissa, joissa on useita tilauksia. 

5. Kun prosessi on valmis, Azure Security Center Power BI-koontinäytön Lataa alla kuvatun raportissa **Hallinnan** kanssa:

    ![Käytännön hallinnan Raporttinäkymää](./media/security-center-powerbi/security-center-powerbi-fig1-new9.png)

## <a name="see-also"></a>Katso myös
Tässä asiakirjassa opit, voit käyttää Power BI Azure Tietoturvakeskuksessa. Saat lisätietoja Azure Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Azure Security Center suunnittelu- ja käyttöoppaan](security-center-planning-and-operations-guide.md) – Opettele suunnittelemaan Azure Tietoturvakeskus käyttöönoton.
- [Azure Tietoturvakeskuksessa suojauskäytäntöjen määrittäminen](security-center-policies.md) – Opi Azure Tietoturvakeskus tietosuoja-asetusten määrittäminen
- [Hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md) – Katso, miten voit hallita ja vastata suojausvaroitusten
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md) – Etsi usein kysytyt kysymykset-palvelun avulla
- [Azure Security-blogi](http://blogs.msdn.com/b/azuresecurity/) – Etsi tietoja Azure tietosuojaan ja vaatimustenmukaisuuteen blogit
