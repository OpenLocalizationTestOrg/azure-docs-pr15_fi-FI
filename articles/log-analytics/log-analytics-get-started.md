<properties
    pageTitle="Aloittaminen Log Analytics | Microsoft Azure"
    description="Saa käytön Log Analytics-Microsoft toimintojen hallinta Suite (OMS) minuutteina."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="get-started-with-log-analytics"></a>Lokitiedoston Analytics käytön aloittaminen

Saa käytön Log Analytics-Microsoft toimintojen hallinta Suite (OMS) minuutteina. Kun valitset luomisesta OMS-työtila, joka on samankaltainen kuin tilin on kaksi vaihtoehtoa:

- Microsoft toimintojen hallinta Suite sivuston
- Microsoft Azure-tilaus

Voit luoda vapaa OMS-työtilan OMS-sivuston käyttäminen. Tai voit käyttää Microsoft Azure tilauksen OMS-työtilan luominen. Molemmat työtilat toiminnot ovat samat kuin, paitsi että vapaa OMS työtilan lähettää vain 500 Megatavua tietoa päivittäin OMS-palveluun. Jos käytössäsi on Azure-tilaus, voit myös käyttää Azure muiden kyseisen tilauksen avulla. Riippumatta siitä, jonka avulla työtilan luominen menetelmä luo työtilan Microsoft-tili tai organisaation tiliä.

Seuraavassa on prosessin Katsaus:

![onboarding kaavio](./media/log-analytics-get-started/oms-onboard-diagram.png)

## <a name="log-analytics-prerequisites-and-deployment-considerations"></a>Kirjaudu Analytics edellytykset ja käyttöönottoon liittyviä huomioita

- Tarvitset Microsoft Azure maksulliseen tilaukseen käyttämään lokitiedostojen Analytics kokonaan. Jos sinulla ei ole Azure tilauksen, luo [ilmainen tili](https://azure.microsoft.com/free/) , jolla voit käyttää Azure-palvelua. Vaihtoehtoisesti voit luoda ilmaisen OMS tilin [Toimintojen hallinta Suite](http://microsoft.com/oms) sivustosta ja valitse **Kokeile ilmaiseksi**.
- OMS-työtila
- Kunkin Windows-tietokoneessa, johon haluat kerätä tietoja Suorita Windows Server 2008: n SP1 tai uudempi
- [Palomuurin](log-analytics-proxy-firewall.md) käyttöoikeus OMS WWW-palvelun osoitteet
- [OMS Log Analytics Forwarder](https://blogs.technet.microsoft.com/msoms/2016/03/17/oms-log-analytics-forwarder) (yhdyskäytävää)-palvelimeen, voit lähettää liikenne palvelimien OMS, jos Internet-yhteyttä ei ole käytettävissä tietokoneista
- Jos käytät Operations Manager Log Analytics tukee Operations Manager 2012 SP1 UR6 ja yläpuolella ja Operations Manager 2012 R2 UR2 ja suurempia. Välityspalvelimen tuki on lisätty Operations Manager 2012 SP1 UR7 ja Operations Manager 2012 R2 UR3. Määrittää, miten se voi integroida OMS.
- Selvitä, jos tietokoneissa on suora Internet-yhteys. Jos näin ei ole, ne vaativat yhdyskäytäväpalvelimen OMS WWW-palvelun sivustojen. Kaikki käyttöä on https.
- Määrittää, mitkä toiminnot ja palvelinten lähettää tietoja OMS. Esimerkiksi toimialueen ohjaimet, SQL Serveristä, jne.
- OMS ja Azure käyttäjille myönnettävät käyttöoikeudet.
- Jos olet huolissasi tietoliikenteestä, kunkin ratkaisun yksitellen käyttöönottoa ja testaa vaikutus suorituskykyyn ennen kuin lisäät lisäresursseja.
- Tarkastele tietoliikenteestä ja suorituskyvyn, kun lisäät Log Analytics ratkaisuja ja toiminnot. Tämä sisältää tapahtuman sivustokokoelman, lokitietojen keräämistä, tietojen kerääminen suorituskyvyn. Se on paremmin aloittaa mahdollisimman vähän sivustokokoelman, kunnes tietoliikenteestä tai suorituskykyyn vaikuttavia yksilöity.
- Varmista, että Windows tekijöiden ei myös hallita käyttämällä Operations Manager, muuten kaksoisarvoja tuloksena. Tämä koskee myös Azure-pohjainen-agenttien vuoksi, joissa on käytössä Azure Diagnostiikka.
- Kun olet asentanut agenttien vuoksi, tarkista, että agentti toimii oikein. Jos ei tarkista varmistaa, että salaus API: Seuraava luonti (CNG) avaimen eristystaso ei ole poistettu käytöstä ryhmäkäytännön avulla.
- Jotkin Log Analytics-ratkaisut on lisävaatimukset



## <a name="sign-up-in-3-steps-using-the-operations-management-suite"></a>Vaiheessa 3 toimintojen hallinta Suite rekisteröintiä

1. [Toimintojen hallinta Suite](http://microsoft.com/oms) -sivustossa ja valitse **Kokeile ilmaiseksi**. Kirjaudu sisään Microsoft-tiliä, kuten Outlook.com tai organisaatiotilillä myöntämä yritys tai oppilaitos käyttää Office 365: ssä tai muiden Microsoftin palvelujen kanssa.
2. Anna yksilöllinen työtilan nimi. Työtilan on looginen säilö hallintatietojen tallennussijainti. Sen avulla osion tietojen eri ryhmille organisaation tapa, kun tiedot ovat yksityisesti työtilaan. Määritä sähköpostiosoite ja alue, johon haluat tallentaa tiedot.  
    ![Työtilan luominen ja linkittää tilaus](./media/log-analytics-get-started/oms-onboard-create-workspace-link01.png)
3. Seuraavaksi voit luoda uuden Azure-tilauksen tai linkki Azure olemassa olevaan tilaukseen. Jos haluat jatkaa maksuttoman kokeiluversion avulla, valitse **Nyt**.  
  ![Työtilan luominen ja linkittää tilaus](./media/log-analytics-get-started/oms-onboard-create-workspace-link02.png)

Olet valmis aloittamaan toimintojen hallinta Suite-portaalissa.

Saat lisätietoja työtilan määrittämisestä ja linkittäminen aiemmin Azure tilit, jotka on luotu toimintojen hallinta Suite osoitteessa [Log Analytics käyttöoikeuksien hallinta](log-analytics-manage-access.md).

## <a name="sign-up-quickly-using-microsoft-azure"></a>Rekisteröi käyttäen nopeasti Microsoft Azure

1. Siirry [Azure portal](https://portal.azure.com) ja kirjaudu sisään, Selaa palveluluetteloon ja valitse sitten **Log Analytics (OMS)**.  
    ![Azure portal](./media/log-analytics-get-started/oms-onboard-azure-portal.png)
2. Valitse **Lisää**ja valitse sitten seuraavat vaihtoehdot:
    - **OMS työtilan** nimi
    - **Tilauksen** – Jos sinulla on useita tilauksia, valitse jokin haluat yhdistää uuteen työtilaan.
    - **Resurssiryhmä**
    - **Sijainti**
    - **Hinnat taso**  
        ![nopea luominen](./media/log-analytics-get-started/oms-onboard-quick-create.png)
3. Valitse **Luo** ja näet työtilan tiedot Azure-portaalissa.       
    ![työtilan tiedot](./media/log-analytics-get-started/oms-onboard-workspace-details.png)         
4. Valitse Avaa Toimintojen hallinta Suite-sivuston kanssa uuden työtilan **OMS Portal** -linkki.

Olet valmis aloittamaan toimintojen hallinta Suite-portaalissa.

Saat lisätietoja työtilan määrittämisestä ja linkittäminen aiemmin työtilat, jotka olet luonut toimintojen hallinta Suite Azure-tilauksia, [loki Analytics käyttöoikeuksien hallinta](log-analytics-manage-access.md).

## <a name="get-started-with-the-operations-management-suite-portal"></a>Toimintojen hallinta Suite-portaalin käytön aloittaminen
Ratkaisuja ja yhdistää palvelimissa, jotka haluat hallita, valitse **asetukset** -ruutu ja noudata tämän osan vaiheet.  

![käytön aloittaminen](./media/log-analytics-get-started/oms-onboard-get-started.png)  

1. **Lisää ratkaisut** - tarkastella asennettu ratkaisuja.  
    ![ratkaisut](./media/log-analytics-get-started/oms-onboard-solutions.png)  
    Valitse Lisää ratkaisuja lisäämään **käy valikoiman** .  
    ![ratkaisut](./media/log-analytics-get-started/oms-onboard-solutions02.png)  
    Valitse ratkaisu ja valitse sitten **Lisää**.
2. **Yhdistä lähde** - Valitse, miten haluat muodostaa yhteyden server-ympäristössä voit kerätä tietoja:
    - Yhdistä Windows Server- tai asiakkaan suoraan asentamalla agentti.
    - Yhdistä Linux-palvelimet OMS-agentti Linux varten.
    - Käytä Windows- tai Linux Azure diagnostiikan AM tunniste on määritetty Azure-tallennustilan tilin.
    - System Center Operations Manager avulla voit liittää hallinta-ryhmiä tai koko Operations Manager käyttöönoton.
    - Ota käyttöön Windowsin käyttämään päivittäminen analyysin Telemetriatietojen.
        ![yhdistettyjen lähteiden](./media/log-analytics-get-started/oms-onboard-data-sources.png)    

3. **Kerää tiedot** Määritä vähintään yksi tietolähde työtilan tietojen täyttämiseen. Kun olet valmis, valitse **Tallenna**.    

    ![kerää tiedot](./media/log-analytics-get-started/oms-onboard-logs.png)    


## <a name="optionally-connect-servers-directly-to-the-operations-management-suite-by-installing-an-agent"></a>Voit myös yhdistää palvelinten suoraan toimintojen hallinta ohjelmistopakettiin asentamalla agentti

Seuraavassa esimerkissä esitetään asentaminen Windows-agentti.

1. Valitse **asetukset** -ruutu, **Yhdistetty tietolähteet** -välilehti, valitse lähteen tyyppi, johon haluat lisätä ja joko Lataa-välilehti agentti tai ottamisesta käyttöön agentti tietoja. Valitse esimerkiksi **Lataaminen Windows-agentti (64-bittinen)**. Windowsin agenttien vuoksi voit asentaa vain anonyymi, Windows Server 2008: n SP 1 tai myöhemmin tai Windows 7 SP1 tai uudempi.
2. Asenna agentti vähintään yksi palvelimiin. Voit asentaa tekijöiden yksi kerrallaan tai [mukautettu komentosarja](log-analytics-windows-agents.md)- tai Lisää automaattinen menetelmä käyttöä käyttää aiemman ohjelmiston jakauman ratkaisun, joka voi olla.
3. Kun asiakas hyväksyy käyttöoikeussopimuksen ehdot ja valitset asennuskansion, valitse **Muodosta agent Azure Log Analytics (OMS) avulla**.   
    ![edustajan määrittäminen](./media/log-analytics-get-started/oms-onboard-agent.png)

4. Valitse seuraavalla sivulla sinua pyydetään työtila-tunnus ja työtila-näppäintä. Työtila-tunnus ja avaimen näkyvät näytössä, johon olet ladannut agent-tiedosto.  
    ![agentti näppäimet](./media/log-analytics-get-started/oms-onboard-mma-keys.png)  

    ![Liitä palvelimet](./media/log-analytics-get-started/oms-onboard-key.png)
5. Asennuksen aikana valitsemalla **Lisäasetukset** voit myös määrittää välityspalvelimeen ja antamalla todennustiedot. Valitse palaa työtilan asetukset-sivulla **Seuraava** -painiketta.
6. Valitse **Seuraava** vahvistamiseen työtila-tunnus ja avaimen avulla. Jos virheitä löytyy, voit valita **takaisin** korjausten. Kun työtila-tunnus ja avaimen avulla tarkistetaan, valitse agent-asennuksen viimeistelemiseen **asentaminen** .
7. Valitse Ohjauspaneelissa Microsoft Agent seuranta > Azure Log Analytics (OMS)-välilehti. Vihreä valintamerkki-kuvake tulee näkyviin, kun niistä viestinnän toimintoja Management Suite-palvelussa. Aluksi tämä kestää noin 5 – 10 minuuttia.

>[AZURE.NOTE] Kapasiteetin hallinta- ja määritystietoja arviointi ratkaisuja ei tueta tällä hetkellä kytketystä toimintojen hallinta Suite-palvelimet.


Voit myös muodostaa agentti System Center Operations Manager 2012 SP1 ja uudempi versio. Valitse **Muodosta agent, System Center Operations Manager**tekemään niin. Jos valitset, että vaihtoehto tietojen lähettäminen palvelun tarvitsematta erillistä laitetta tai ladata hallinta-ryhmiä.

Voit lukea lisää tietoja yhteyden muodostamisesta [yhteyden Windows](log-analytics-windows-agents.md)-tietokoneiden, loki Analytics toimintojen hallinta-ohjelmistopaketin agenttien vuoksi.

## <a name="optionally-connect-servers-using-system-center-operations-manager"></a>Voit myös yhdistää System Center Operations Manager käyttävien palvelimien

1. Valitse Operations Manager-konsolin **hallinta**.
2. Laajenna **Toiminnallisia tiedot** -solmu ja valitse **Toiminnallisia tietoyhteyden**.

  >[AZURE.NOTE] Sen mukaan, mitä Update Rollup, SCOM käytössäsi on näkyviin voi tulla solmu *System Center Advisor*, *Toiminnalliset tiedot*tai *Toimintojen hallinta Suite*.

3. Kohti oikeasta yläkulmasta **toiminnallisia havainnollistamisen Rekisteröi** -linkkiä ja noudata ohjeita.
4. Kun olet suorittanut ohjattua rekisteröimistä, valitse **Lisää tietokoneen Ryhmäsarakkeisiin** -linkki.
5. **Tietokoneen Etsi** -valintaikkunassa voit etsiä tietokoneita tai ryhmiä valvoo Operations Manager. Valitse tietokoneiden tai ryhmien määrän ne Log Analytics, valitse **Lisää**ja valitse sitten **OK**. Voit varmistaa, että OMS-palvelun lähetetään siirtymällä toimintojen hallinta Suite portaalissa **käyttö** -ruudun tiedot. Tietojen pitäisi näkyä noin 5 – 10 minuuttia.

Voit lukea lisää tietoja yhteyden muodostamisesta Operations Manager toimintojen hallinta Suite [Yhteyden Operations Manager Log Analytics](log-analytics-om-agents.md)-palvelussa.

## <a name="optionally-analyze-data-from-cloud-services-in-microsoft-azure"></a>Voit myös analysoida cloud Services-palveluista Microsoft Azure

Toimintojen hallinta Suite voit etsiä nopeasti tapahtuma- ja IIS-lokeja pilvipalveluihin ja näennäiskoneiden ottamalla Azure Cloud-palveluiden Diagnostiikka. Näyttöön tulee Lisää tiedot myös Azuren näennäiskoneiden asentamalla Microsoft Agent seuranta. Voit lukea lisää Azure ympäristön käyttää toimintojen hallinta-ohjelmistopaketin [Yhdistäminen Azure tallennustilan Log Analytics](log-analytics-azure-storage.md)määrittämisestä.


## <a name="next-steps"></a>Seuraavat vaiheet

- [Lisää Log Analytics-ratkaisuja ratkaisuvalikoimasta](log-analytics-add-solutions.md) Lisää toimintoja ja kerätä tietoja.
- Tutustu [log haut](log-analytics-log-searches.md) haluat tarkastella yksityiskohtaisia tietoja keräämien ratkaisuja.
- [Raporttinäkymien](log-analytics-dashboards.md) avulla voit tallentaa ja näyttää omia mukautettuja hakuja.
