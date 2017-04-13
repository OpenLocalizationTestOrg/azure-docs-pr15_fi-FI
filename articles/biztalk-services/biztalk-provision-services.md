<properties
    pageTitle="Luo Azure BizTalk palvelua Azure-portaalissa | Microsoft Azure"
    description="Lue, miten voit valmistella tai luo Azure BizTalk Services Azure-portaalista; MAB-WABS"
    services="biztalk-services"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/15/2016"
    ms.author="mandia"/>



# <a name="create-biztalk-services-using-the-azure-portal"></a>Luo BizTalk palvelua Azure-portaalissa

Luo Azure BizTalk Services Azure-portaalissa.

> [AZURE.TIP] Kirjaudu Azure-portaaliin, tarvitset Azure-tili ja Azure tilauksen. Jos sinulla ei ole tiliä, voit luoda maksuttoman kokeiluversion tilin muutaman minuutin kuluessa. Katso [Azure maksuttoman kokeiluversion](http://go.microsoft.com/fwlink/p/?LinkID=239738).

## <a name="create-a-biztalk-service"></a>Luo BizTalk-palvelu
Voit valita Edition mukaan kaikki BizTalk Palveluasetukset eivät ole käytettävissä.

1. Kirjautuminen [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Valitse ala-siirtymisruudussa **Uusi**:  
![Valitse uusi-painike][NEWButton]

3. Valitse **Sovelluksen palvelut** > **BIZTALK palvelun** > **MUKAUTETUN luominen**:  
![Valitse BizTalk palvelu ja valitse mukautettu luominen][NewBizTalkService]

4. Kirjoita BizTalk palvelun asetukset:

    <table border="1">
    <tr>
    <td><strong>BizTalk palvelunimi</strong></td>
    <td>Voit kirjoittaa haluamasi nimen, mutta ole täsmällinen. Esimerkkejä:<br/><br/>
    <em>yritys</em>. biztalk.windows.net<br/>
    <em>mycompanymyapplication</em>. biztalk.windows.net<br/>
    <em>myapplication</em>. biztalk.windows.net<br/><br/>". biztalk.windows.net" lisätään automaattisesti kirjoittamasi nimi. Tämä luo URL-osoite, jota käytetään käyttää BizTalk-palvelua, kuten <strong>https://<em>myapplication</em>. biztalk.windows.net</strong>.
    </td>
    </tr>
    <tr>
    <td><strong>Edition</strong></td>
    <td>Jos olet testausta kehitystä/vaiheessa, valitse <strong>Kehitystyökalut</strong>. Jos olet avannut tuotannon vaihe, käytä <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">BizTalk Services: versiot kaavion</a> , onko <strong>Premium</strong>, <strong>Normaali</strong>tai <strong>Basic</strong> business-skenaarion oikea valinta.
    </td>
    </tr>
    <tr>
    <td><strong>Alue</strong></td>
    <td>Valitse maantieteellisen alueen isännöimiseen BizTalk-palvelussa.</td>
    </tr>
    <tr>
    <td><strong>Toimialueen URL-osoite</strong></td>
    <td><strong>Valinnainen</strong>. Toimialueen URL-osoite on oletusarvoisesti <em>YourBizTalkServiceName</em>. biztalk.windows.net. Mukautetun toimialueen myös voidaan kirjoittaa. Esimerkiksi jos toimialue on <em>contoso</em>, voit kirjoittaa: <br/><br/>
    <em>Yritys</em>. contoso.com<br/>
    <em>MyCompanyMyApplication</em>. contoso.com<br/>
    <em>MyApplication</em>. contoso.com<br/>
    <em>YourBizTalkServiceName</em>. contoso.com<br/>
    </td>
    </tr>
    </table>
Valitse seuraava-nuoli.

5. Lisää tallennustilaa ja tietokannan asetukset:

    <table border="1">
    <tr>
    <td><strong>Seuranta/arkistointi-tallennustilan tilin</strong></td>
    <td>Valitse käytössä olevan tallennustilan-tilin tai luoda uuden tallennustilan tilin. <br/><br/>Jos luot uuden tallennustilan tilin, kirjoita <strong>Tallennustilan tilin nimi</strong>.</td>
    </tr>
    <tr>
    <td><strong>Seurantatietokanta</strong></td>
    <td>Jos käytät aiemmin luotuun Azure SQL-tietokantaan, sitä ei voi käyttää toisen BizTalk-palvelun. Tarvitset kirjautumisnimi ja salasana syötetty kun Azure SQL-tietokantapalvelin, johon on luotu.<br/><br/><strong>Vihje</strong> Luo seuranta tietokannan ja näyttö ja arkistointi-tallennustilan tilin BizTalk palveluna samalla alueella.</td>
    </tr>
    </table>
Valitse seuraava-nuoli.

6. Kirjoita tietokannan asetuksia:

    <table border="1">
    <tr>
    <td><strong>Nimi</strong></td>
    <td>Käytettävissä, kun <strong>Luo uuden esiintymän SQL-tietokanta</strong> on valittu edelliseen näyttöön.
    <br/><br/>
   Kirjoita käytettävän BizTalk-palvelu SQL-tietokannan nimi.</td>
    </tr>
    <tr>
    <td><strong>Palvelin</strong></td>
    <td>Käytettävissä, kun <strong>Luo uuden esiintymän SQL-tietokanta</strong> on valittu edelliseen näyttöön.
    <br/><br/>
   Valitse SQL-tietokantaan tai Luo uusi SQL-tietokantapalvelimeen.</td>
    </tr>
    <tr>
    <td><strong>Palvelimen käyttäjätunnus</strong></td>
    <td>Kirjoita käyttäjän kirjautumisnimi.</td>
    </tr>
    <tr>
    <td><strong>Palvelimen kirjautumissalasana</strong></td>
    <td>Kirjaudu sisään salasana.</td>
    </tr>
    <tr>
    <td><strong>Alue</strong></td>
    <td>Käytettävissä, kun <strong>Luo uuden esiintymän SQL-tietokanta</strong> on valittuna. Valitse maantieteellisen alueen isännöi SQL-tietokantaan.</td>
    </tr>
    </table>

Valitse Suorita ohjattu valintamerkkiä. Edistyminen-kuvake tulee näkyviin:  
![Edistymisen kuvakkeessa näkyy, kun olet valmis][ProgressComplete]

Kun olet valmis, Azure BizTalk-palvelu on luotu ja valmis sovellusten. Oletusasetukset on riittävä. Jos haluat muuttaa oletusasetuksia, valitse **BIZTALK palvelut** vasemmassa siirtymisruudussa ja valitse sitten BizTalk-palvelussa. Muut asetukset näkyvät yläreunassa [raporttinäkymät-ikkunan, näyttö- ja skaalaus-välilehdet](biztalk-dashboard-monitor-scale-tabs.md) .

BizTalk-palvelun tilan mukaan on joitakin toiminnoista, joita ei voi suorittaa loppuun. Luettelo toiminnoista Siirry [BizTalk palvelujen tila kaavioon](biztalk-service-state-chart.md).


## <a name="post-provisioning-steps"></a>Valmistelu jälkeiset toimet

-  [Varmenteen asentaminen paikalliseen tietokoneeseen](#InstallCert)
-  [Lisää tuotannon käyttövalmiin varmenne](#AddCert)
-  [Hae nimitilan käyttöoikeuksien valvonta](#ACS)

#### <a name="InstallCert"></a>Varmenteen asentaminen paikalliseen tietokoneeseen
Itse allekirjoitetun varmenteen luodaan ja BizTalk Service-tilaukseen liittyvää osana BizTalk palvelun valmistelussa. On tämän todistuksen Lataa ja asenna se tietokoneissa, jossa voit käyttöönotto BizTalk Palvelusovellusten tai lähettää viestejä BizTalk palvelun päätepiste.

1. Kirjautuminen [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Valitse vasemmassa siirtymisruudussa **BIZTALK palvelut** ja valitse sitten tilauksen BizTalk palvelun.
3. Valitse **koontinäyttö** -välilehti.
4. Valitse **Lataa SSL-varmenne**:  
![SSL-varmenteen muokkaaminen][QuickGlance]
5. Kaksoisnapsauta varmennetta ja suorita ohjattu toiminto sertifikaatin asennus. Varmista, että olet asentanut **Luotettu varmenteen päämyöntäjien** säilöön-kohdan varmennetta.

#### <a name="AddCert"></a>Lisää tuotannon käyttövalmiin varmenne
Itse allekirjoitetun varmenteen, jotka luodaan automaattisesti, kun BizTalk palvelujen luomista on tarkoitettu käytettäväksi vain kehittäminen ympäristöissä. Tuotannon tilanteessa korvaa se tuotannon käyttövalmiin varmenne.

1. **Raporttinäkymät-ikkunan** -välilehdessä **Päivitä SSL-varmenne**.
2. Siirry oman yksityinen SSL-varmenne (*CertificateName*.pfx), joka sisältää BizTalk palvelun nimi, salasana ja valitse sitten valintamerkkiä.

#### <a name="ACS"></a>Hae nimitilan käyttöoikeuksien valvonta

1. Kirjautuminen [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Valitse vasemmassa siirtymisruudussa **BIZTALK palvelut** ja valitse sitten BizTalk-palvelussa.
3. Valitse tehtäväpalkissa **Yhteyden tietoja**:  
![Valitse yhteystiedot][ACSConnectInfo]

4. Kopioi käyttöoikeuksien valvonta-arvot.

Kun otat käyttöön Visual Studiossa BizTalk palvelun projektin, voit kirjoittaa käyttöoikeuksien valvonta-nimitilaa. Käyttöoikeuksien hallinta-nimitilan luodaan automaattisesti BizTalk-palveluun.

Käyttöoikeuksien hallinta-arvot voidaan on kaikissa sovelluksissa. Azure BizTalk Services luomisen jälkeen käyttöoikeuksien valvonta-Nimitilan ohjaa todennus BizTalk palvelun käyttöönoton kanssa. Jos haluat muuttaa tilauksen tai hallita nimitilan, valitse **ACTIVE DIRECTORY** vasemmassa siirtymisruudussa ja valitse sitten oman nimitilan. Tehtäväpalkista luettelo oleviin vaihtoehtoihin.

Access-ohjausobjektin hallinta-portaalin valitsemalla **Hallitse** avautuu. Accessin ohjausobjektin hallinta-portaalin BizTalk-palvelun käyttää **palvelun käyttäjätietojen**:  
![ACS palvelun käyttäjätietojen Accessin hallinta-portaalin hallinta][ACSServiceIdentities]

Käytönvalvonta palvelun tunnistetiedot on joukko tunnistetiedot, jotka Salli sovellusten tai asiakkaasi todentamismenetelmä käytönvalvonta ja vastaanottaa tunnusta.

> [AZURE.IMPORTANT]BizTalk-palvelun käyttää **omistaja** oletusarvo-palvelun käyttäjätieto- ja **salasana** -arvo. Seuraava virhe voi ilmetä, jos käytät symmetrisen avaimen arvon sen sijaan, että salasana-arvo.<br/><br/>*Määritetyn tunnuksilla Access-ohjausobjektin hallintapalvelu-tiliä ei voitu muodostaa*

[Hallinta Your ACS Namespace](https://msdn.microsoft.com/library/azure/hh674478.aspx) on lueteltu joitakin suuntaviivoja ja suositukset.

## <a name="requirements-explained"></a>Kuvaus vaatimukset

Nämä vaatimukset eivät koske vapaa Edition.
<table border="1">
<tr bgcolor="FAF9F9">
        <td><strong>Tarvitset</strong></td>
        <td><strong>Miksi sitä tarvitaan</strong></td>
</tr>
<tr>
<td>Azure-tilaus</td>
<td>Tilauksen määrittää, ketkä voivat kirjautua sisään Azure-portaaliin. Tilin omistajan Luo tilaus <a HREF="https://account.windowsazure.com/Subscriptions">Azure-tilauksia</a>.
<br/><br/>
Azure-tili voi olla useita tilauksia, ja se voi hallita kuka tahansa, jolla sallitaan. Esimerkiksi Azure-tili haltijan Luo tilausta nimeltä <em>BizTalkServiceSubscription</em> ja antaa BizTalk-järjestelmänvalvojien yrityksen (esimerkiksi ContosoBTSAdmins@live.com) access tähän tilaukseen. Tässä skenaariossa BizTalk järjestelmänvalvojat Kirjaudu Azure-portaaliin ja saada koko järjestelmänvalvojan oikeudet isännöityjen palveluiden tilauksen Azure BizTalk palveluja. BizTalk-järjestelmänvalvojat eivät ole haltijan Azure-tili ja näin ollen eivät voi käyttää mitään tietoja.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577">Tilausten hallinta ja tallennustilaa tilit Azure-portaalissa</a> on lisätietoja.
</td>
</tr>
<tr>
<td>Azure SQL-tietokanta</td>
<td>Tallentaa taulukoiden, näkymien ja tallennettujen toimintosarjojen käyttämä BizTalk-palvelua, mukaan lukien seuranta-tiedot.
<br/><br/>
Kun luot BizTalk palvelu, voit käyttää aiemmin Azure SQL Serveriä, Azure SQL-tietokantaan, tai luo automaattisesti uuden palvelimen tai tietokannan.
<br/><br/>
SQL-tietokanta-asteikkoa määritetään automaattisesti. Oletus-asteikkoa on yleensä sovellu BizTalk palvelu. Asteikon muuttaminen vaikuttaa hinnoittelua. Katso <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930">tilit ja laskutuksen Azure SQL-tietokantaan</a>
<br/><br/>
<strong>Huomautuksia</strong>
<br/>
<ul>
<li> Kun luot uuden Azure SQL-palvelin ja tietokanta, Azure palvelut otetaan automaattisesti käyttöön. BizTalk-palvelu edellyttää Azure-palvelut on otettu käyttöön.</li>
<li>Jos luot uuden Azure SQL-tietokannan aiemmin luotuun Azure SQL Server-palvelimen palomuurisäännöt eivät muutu. Tuloksena on mahdollista Azure muut palvelut eivät ole sallittuja palvelimen tietokantoja.</li>
</ul>
</td>
</tr>
<tr>
<td>Azure käytönvalvonta nimitila</td>
<td>Todentaa Azure BizTalk-palvelujen kanssa. Kun otat käyttöön Visual Studiossa BizTalk palvelun projektin, voit kirjoittaa käyttöoikeuksien valvonta-nimitilaa. Kun luot BizTalk palvelu, käyttöoikeuksien valvonta-Nimitilan luodaan automaattisesti.</td>
</tr>

<tr>
<td>Azure-tallennustilan tilin</td>
<td>Antaa pääsy taulukoiden, BLOB-objektit ja olevien BizTalk-palvelun käyttämä Tallenna seuraavasti:

<ul>
<li>Log-tiedostot, jotka voit valvoa BizTalk-palvelua. Seurannan tulos näkyy myös Azure-portaalissa **Seuranta** -välilehti.</li>
<li>Luodessasi X12 tai AS2 sopimus kumppanit, voit ottaa arkistointi-toiminto, jolla tallentaa viestin ominaisuuksia. Tiedot tallennetaan-tallennustilan tilin.</li>
</ul>
<br/>
Kun luot BizTalk palvelu, voit käyttää käytössä olevan tallennustilan-tilin tai luo automaattisesti uuden tallennustilan tilin.
<br/><br/>
Tallennustilan oletusasetukset ovat riittävän BizTalk-palveluun.
<br/><br/>
Kun luot tallennustilan tilin, perusavain ja toissijaisen avaimen luodaan automaattisesti. Painettavat näppäimet hallita tallennustilan-tiliisi. BizTalk-palvelu käyttää perusavaimen automaattisesti.
<br/><br/>
Saat lisätietoja <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671">tallennustilan</a> .
</td>
</tr>

<tr>
<td>Yksityinen SSL-varmenne</td>
<td>
Azure-BizTalk palvelun luomisen jälkeen BizTalk palvelun nimi HTTPS URL-osoite luodaan. Tätä URL-Osoitetta on automaattisesti määritetty käyttämään vain kehittäminen itse allekirjoitettua varmennetta. Tuotannon sinun on yksityinen SSL-varmenne.
<br/><br/>
<strong>Tärkeää SSL-varmenteen tiedot</strong>

<ul>
<li>Sertifikaatin vanhentumispäivä on oltava pienempi kuin 5 vuotta.</li>
<li>Salasanan vaatiminen kaikki yksityinen varmenteet. Tämä salasana ja paras käytäntö on jakaa salasana oman järjestelmänvalvojat.</li>
<li>Itse allekirjoitetun varmenteita käytetään testi/kehitysympäristö. Itse allekirjoitetun varmenteet käytettäessä henkilökohtaisen varmenteen kaupan ja luotettujen päämyöntäjien varmenteen kauppa-varmenteen tuominen.</li>
</ul>
<br/>Kun lähetät tuotannon varmenteen pyynnön varmenteen myöntäjältä, antaa varmenteen seuraavat ominaisuudet:
<br/>

<ul>
<li><strong>Laajennettu avaimen käyttö</strong>: vähintään Azure BizTalk Services edellyttää palvelimen todennuksen.</li>
<li><strong>Yleinen nimi</strong>: Kirjoita Azure BizTalk-palvelun URL-osoite täydellinen toimialuenimi (FQDN). Tämän artikkelin kohdassa <a HREF="#BizTalk">Luo BizTalk palvelu</a> .</li>
</ul>
<br/>
Uuden tai erilaisen varmenteen voidaan lisätä BizTalk-palvelun luomisen jälkeen.
</td>
</tr>
</table>



## <a name="hybrid-connections"></a>Hybrid yhteydet

Kun luot Azure-BizTalk palvelu, **Hybrid yhteydet** -välilehti on käytettävissä:

![Hybrid yhteydet-välilehti][HybridConnectionTab]

Hybrid yhteyksiä käytetään yhteyden Azure-sivustossa tai Azure mobiilipalvelu mitä tahansa paikallista resurssia, joka käyttää staattiset porttinumeroa, kuten SQL Server, MySQL, HTTP-verkko-ohjelmointirajapinnan, Mobile-palveluihin ja useimmat mukautetun verkkopalvelut.  Hybrid yhteydet ja BizTalk sovittimen palvelun eroavat toisistaan. BizTalk sovittimen palvelua käytetään yhteyden Azure BizTalk Services rivi, Business ALUESOVELLUKSISTA paikalliseen järjestelmään.

 Katso lisätietoja, kuten luomisen ja ylläpidon Hybrid yhteydet [Hybrid yhteydet](integration-hybrid-connection-overview.md) .


## <a name="next-steps"></a>Seuraavat vaiheet

BizTalk-palvelu on luotu, tutustu eri [BizTalk Services: Raporttinäkymät-ikkunan, näyttö ja skaalaus-välilehdillä](biztalk-dashboard-monitor-scale-tabs.md). BizTalk-palvelu on valmiina sovellukset. Aloita sovellusten luominen, siirry [Azure BizTalk palvelut](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Katso myös
- [BizTalk Services: Versiot kaavio](biztalk-editions-feature-chart.md)<br/>
- [BizTalk Services: Tilan kaavio](biztalk-service-state-chart.md)<br/>
- [BizTalk Services: Varmuuskopiointi ja palauttaminen](biztalk-backup-restore.md)<br/>
- [BizTalk Services: rajoittaminen](biztalk-throttling-thresholds.md)<br/>
- [BizTalk Services: Myöntäjän nimi ja myöntäjä avain](biztalk-issuer-name-issuer-key.md)<br/>
- [Miten voin aloittaa käyttämällä Azure BizTalk Services SDK-paketissa](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
- [Hybrid yhteydet](integration-hybrid-connection-overview.md)

[NewBizTalkService]: ./media/biztalk-provision-services/WABS_NewBizTalkService.png
[NEWButton]: ./media/biztalk-provision-services/WABS_New.png
[ProgressComplete]: ./media/biztalk-provision-services/WABS_ProgressComplete.png
[ACSConnectInfo]: ./media/biztalk-provision-services/WABS_ACSConnectInformation.png
[QuickGlance]: ./media/biztalk-provision-services/WABS_QuickGlance.png
[ACSServiceIdentities]: ./media/biztalk-provision-services/WABS_ACSServiceIdentities.png
[HybridConnectionTab]: ./media/biztalk-provision-services/WABS_HybridConnectionTab.png
