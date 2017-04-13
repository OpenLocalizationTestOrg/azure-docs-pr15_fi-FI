<properties
   pageTitle="Siirtyminen Orchestrator Azure automaatio | Microsoft Azure"
   description="Tässä artikkelissa käsitellään runbooks ja integrointi siirtäminen Azure automaatio System Center Orchestrator."
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="bwren" />


# <a name="migrating-from-orchestrator-to-azure-automation-beta"></a>Siirtyminen Orchestrator Azure automaatio (Beta)

[System Center Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) Runbooks perustuvat integrointi-paketteja, jotka on kirjoitettu Orchestrator varten, kun Azure automaatio runbooks perustuvat Windows PowerShell-toimintoja.  [Graafisen runbooks](automation-runbook-types.md#graphical-runbooks) Azure automaatio-on samanlainen ulkoasu Orchestrator runbooks, ja heidän toimintaansa PowerShell-cmdlet-komennot, lapsen runbooks ja resurssien.

[System Center Orchestrator siirron työkalujen](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) sisältää työkaluja, voit helpottaa runbooks muuntaminen Azure automaatio Orchestrator.  Lisäksi muuntamista runbooks itse, voit muuntaa integrointi Pack-paketit ja toiminnot, jotka runbooks käyttävät integrointi moduulit kanssa Windows PowerShellin cmdlet-komennot.  

Seuraavassa on muuntamisen Orchestrator runbooks Azure automaatio Perusprosessi.  Kaikki nämä vaiheet on kuvattu alla olevissa osioissa tiedot.

1.  Lataa [System Center Orchestrator siirron työkalujen](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) sisältää työkaluja ja tässä artikkelissa kuvatut moduulit.
2.  Tuo Azure automaatio [kuulumattomat moduuli](#standard-activities-module) .  Tämä sisältää vakio Orchestrator toimintoja, jotka voivat käyttää muunnettu runbooks muunnettu versiot.
3.  [System Center Orchestrator integrointi moduulit](#system-center-orchestrator-integration-modules) tuominen Azure automaatio for oman runbooks, joka käyttää System Center käyttämä näiden integrointi Pack-paketit.
4.  Muunna mukautettu ja kolmannen osapuolen [Integrointi Pack muuntimen](#integration-pack-converter) käyttäminen integration-paketit ja Azure automaatio tuominen.
5.  Muuntaa Orchestrator runbooks [Runbookin muuntimen](#runbook-converter) käyttäminen ja asenna Azure automaatio.
6.  Luo manuaalisesti tarvittavat Orchestrator varat Azure automaatio jälkeen Runbookin muunnin ei muunna nämä resurssit.
7.  Määrittää paikallisen tietokeskuksen suorittaa muunnettu runbooks, joka käyttää Paikalliset resurssit [Hybrid Runbookin työntekijä](#hybrid-runbook-worker) .

## <a name="service-management-automation"></a>Palvelun hallinta automatisointi

[Palvelun hallinta automatisointi](http://technet.microsoft.com/library/dn469260.aspx) (SMA) tallentaa ja suorittaa runbooks paikallisen tietokeskuksen kuten Orchestrator ja se käyttää samaa integrointi moduulit Azure automaatio. [Runbookin muuntimen](#runbook-converter) muuntaa Orchestrator runbooks graafinen runbooks vaikka joka SMA ei tueta.  Voit asentaa [Toimintoja perusmoduuli](#standard-activities-module) ja [System Center Orchestrator integrointi moduulit](#system-center-orchestrator-integration-modules) edelleen SMA kyselyjä, mutta sinun täytyy manuaalisesti [uudelleen oman runbooks](http://technet.microsoft.com/library/dn469262.aspx).

## <a name="hybrid-runbook-worker"></a>Hybrid Runbookin työntekijä

Runbooks Orchestrator tallennetun tietokantapalvelin ja suorittaa runbookin palvelimissa, sekä paikalliset tiedot Centerissä.  Azure automaatio Runbooks tallennetaan Azure pilveen ja että paikallisen tietokeskuksen käyttämällä [Hybrid Runbookin työntekijä](automation-hybrid-runbook-worker.md)suorittaa.  Tämä johtuu siitä, miten voit suorittaa runbooks, koska ne on suunniteltu toimimaan paikallisia palvelimia Orchestrator muuntaa.

## <a name="integration-pack-converter"></a>Integrointi Pack muunnin

Integrointi Pack muuntimen muuntaa integrointi-paketteja, jotka on luotu käyttämällä [Orchestrator integrointi työkalujen (OIT)](http://technet.microsoft.com/library/hh855853.aspx) , integrointi moduulit Windows PowerShellin Azure automaatio tai palvelun hallinta automatisointi voidaan tuoda perusteella.  

Kun suoritat integrointi Pack muuntimen, näyttöön tulee ohjatulla, jotta voit valita integrointi (.oip) pack tiedostoa.  Ohjatun toiminnon Valitse sisältyy kyseisen integrointi Pack-pakettiin toimintojen luettelo ja voit valita, jotka siirretään.  Kun olet suorittanut ohjatun toiminnon, Luo integrointi-moduuli, joka sisältää vastaavan cmdlet-komento kunkin alkuperäisen integrointi Pack toimintoja.


### <a name="parameters"></a>Parametrit

Tehtävän integrointi Pack ominaisuuksia muunnetaan vastaavan cmdlet-komento integrointi-moduulin parametrit.  Windows PowerShellin cmdlet-komennot on [yhteiset parametrit](http://technet.microsoft.com/library/hh847884.aspx) , joita voidaan käyttää kaikkien cmdlet-komennot.  Esimerkiksi yksityiskohtainen parametri-aiheuttaa cmdlet-komento, jos haluat siirtää sen toiminta yksityiskohtaisia tietoja.  Cmdlet-komento ei ole ehkä parametri, jonka nimi on sama kuin yleisiä parametrin.  Jos tehtävä on ominaisuutta, jolla on sama nimi kuin yleisiä parametrin, ohjattu toiminto kehottaa sinua antamaan parametrin toinen nimi.

### <a name="monitor-activities"></a>Valvonta-toiminnot

Näytön runbooks Orchestrator- [näytön tehtävä](http://technet.microsoft.com/library/hh403827.aspx) alkaa, ja suorita odottaa jatkuvasti, jota tietty tapahtuma.  Azure automaatio ei tue näytön runbooks, joten integrointi Pack näytön tehtäviä ei muunneta.  Sen sijaan paikkamerkin cmdlet-komento luodaan näytön tehtävän integrointi-moduuli.  Tämä cmdlet-komento ei ole toiminnot, mutta se sallii muunnettu runbookin, joka käyttää on asennettu.  Tämä runbookin voi suorittaa Azure automaatio, mutta se voidaan asentaa, jotta voit muokata sitä.

### <a name="integration-packs-that-cannot-be-converted"></a>Integrointi-paketteja, joka ei voida muuntaa

Integrointi-paketteja, jotka on luotu ei OIT ei voi muuntaa integrointi Pack muuntimen kanssa. Saatavilla on myös osa, joka ei tällä hetkellä voi muuntaa Microsoftin toimittaman tällä työkalulla integrointi-paketeista.  Muunnettu versioissa nämä integrointi Pack-paketit on [tarkoitettu Lataa](#system-center-orchestrator-integration-modules) niin, että ne voidaan asentaa Azure automaatio tai palvelun hallinta automatisointi.


## <a name="standard-activities-module"></a>Kuulumattomat moduuli

Orchestrator sisältää [kuulumattomat](http://technet.microsoft.com/library/hh403832.aspx) , ei sisällytetä integrointi-paketti, mutta monet runbooks avulla.  Kuulumattomat moduuli on integrointi-moduuli, joka sisältää cmdlet-komento, vastaavat kunkin näistä toiminnoista.  Sinun on asennettava integrointi moduuli Azure automaatio, ennen kuin tuot muunnettu runbooks, joka käyttää vakio-toimintoa.

Lisäksi tukevat muunnettu runbooks Cmdlet-komentoja kuulumattomat moduulin voidaan joku tuttuja Orchestrator voit luoda uuden runbooks Azure automaatio.  Kun kaikki kuulumattomat toimintoja voidaan suorittaa suorittamalla cmdlet-komennot, ne voi toimia eri tavalla.  Muunnettu kuulumattomat moduulin cmdlet-komennot toimi sama kuin vastaavan toiminnastaan ja käyttää samaa parametreja.  Tämä voi auttaa niiden siirtymän Azure automaatio runbooks aiemmin Orchestrator runbookin tekijän.

## <a name="system-center-orchestrator-integration-modules"></a>System Center Orchestrator integrointi moduulit

Microsoft toimittaa [integrointi paketteja](http://technet.microsoft.com/library/hh295851.aspx) etsimisen runbooks automatisoida System Center-osat ja muihin ohjelmiin.  Joitain näistä integrointi Pack-paketit perustuvat tällä hetkellä OIT, mutta ei tällä hetkellä voi muuntaa integrointi moduulit tunnettujen ongelmien vuoksi.  [System Center Orchestrator integrointi moduulit](https://www.microsoft.com/download/details.aspx?id=49555) sisältää näitä integrointi-paketteja, jotka voidaan tuoda Azure automaatio ja palvelun hallinta automatisointi muunnettu versiot.  

Tämä työkalu RTM versio julkaistaan integrointi-paketteja, jossa integrointi Pack-muunnin, joka voidaan muuntaa OIT perusteella päivitetyt versiot.  Ohjeet toimitetaan myös voit helpottaa muuntaminen runbooks perustuva OIT integrointi Pack-paketit tehtävien avulla.

## <a name="runbook-converter"></a>Runbookin muunnin

Runbookin muuntimen muuntaa Orchestrator runbooks [Graafinen runbooks](automation-runbook-types.md#graph-runbooks) , joka voi tuoda Azure automaatio.  

Runbookin muuntimen on toteutettu PowerShell-moduulin cmdlet-komennolla kutsutaan **ConvertFrom SCORunbook** , joka suorittaa muuntaminen kanssa.  Kun olet asentanut työkalu, se luo pikakuvakkeen PowerShell-istuntoon, lataa-cmdlet-komennolla.   

Seuraavassa on muuntaa Orchestrator runbookin ja tuoda Azure automaatio Perusprosessi.  Seuraavissa osissa on edelleen tiedot-työkalulla ja muunnettu runbooks käsitteleminen.

1. Vie vähintään yksi runbooks Orchestrator.
2. Hanki integrointi moduulit: n runbookin kaikki toiminnot.
3. Muuntaa vietävän tiedoston Orchestrator-runbooks.
4. Lokien Vahvista muunnos ja määrittämään tarvittavat manuaalinen tehtäviin arviointitietoja.
4. Tuoda muunnetut runbooks Azure automaatio.
5. Luo Azure automaatio kaikki tarvittavat resurssit.
6. Muokkaa runbookin Azure työkalujen muokkaamiseen tarvittavat tehtävät.

### <a name="using-runbook-converter"></a>Runbookin muuntimen käyttäminen

**ConvertFrom SCORunbook** syntaksi on seuraavanlainen:

    ConvertFrom-SCORunbook -RunbookPath <string> -Module <string[]> -OutputFolder <string> 

- RunbookPath - polku vientitiedosto, jossa voit muuntaa runbooks.
- Moduuli - pilkuilla erotettu luettelo integrointi moduuleista sisältävät toimintoja runbooks.
- OutputFolder - kansion polku, voit luoda muuntaa graafinen runbooks. 


Seuraava esimerkkikomento muuntaa runbooks kutsutaan **MyRunbooks.ois_export**Vie tiedostoon.  Nämä runbooks käyttämällä Active Directory- ja tietojen suojauksen hallinta integrointi-paketit.

    ConvertFrom-SCORunbook -RunbookPath "c:\runbooks\MyRunbooks.ois_export" -Module c:\ip\SystemCenter_IntegrationModule_ActiveDirectory.zip,c:\ip\SystemCenter_IntegrationModule_DPM.zip -OutputFolder "c:\runbooks" 


### <a name="log-files"></a>Lokitiedostojen

Runbookin muuntimen luo seuraavat lokitiedostot muunnettu runbookin samaan sijaintiin.  Jos tiedostot ovat jo olemassa, valitse ne korvataan viimeinen muuntaminen tiedoilla.

| Tiedoston | Sisältö |
|:---|:---|
| Runbookin muuntimen - Progress.log | Yksityiskohtaisia ohjeita, mukaan lukien kunkin tehtävän onnistuneesti, joka on muunnettu tiedot ja varoitus jokaiselle tehtävälle ei muunneta muuntamiseen. |
| Runbookin muuntimen - Summary.log  | Yhteenveto viimeisen muuntamiseen, mukaan lukien kaikki varoitukset ja tehtäviä, jotka haluat tehdä esimerkiksi luominen muuttuja, joka on muunnettu runbookin vaaditaan seurantaa.  |

### <a name="exporting-runbooks-from-orchestrator"></a>Runbooks vieminen Orchestrator

Runbookin muuntimen toimii Orchestrator, joka sisältää vähintään yhden runbooks Vie-tiedostosta.  Se luo vientitiedosto vastaavan Azure automaatio-runbookin kunkin Orchestrator runbookin varten.  

Tuominen runbookin Orchestrator, runbookin Runbookin suunnittelussa nimeä hiiren kakkospainikkeella ja valitse **Vie**.  Voit viedä kaikki runbooks kansioon, napsauta kansion nimeä hiiren kakkospainikkeella ja valitse **Vie**.


### <a name="runbook-activities"></a>Runbookin toiminnot

Runbookin muuntimen muuntaa Orchestrator runbookin kunkin tehtävän Azure automaatio-vastaavan aktiviteetti.  Näitä toimintoja, joita ei voi muuntaa paikkamerkin tehtävä luodaan runbookin varoitus tekstillä.  Kun tuot muunnettu runbookin Azure automaatio, nämä toiminnot on tilalle kelvollinen toimintoja, jotka suorittavat tarvittavat toiminnot.

Orchestrator tehtävät [Toiminnot perusmoduuli](#standard-activities-module) muuntuvat.  On vakio joitakin Orchestrator toimintoja, jotka eivät ole Tässä moduulissa kautta, joten ei muunneta.  **Lähetä ympäristö tapahtuman** on esimerkiksi Azure automaatio ei vastaa, koska tapahtuma on Orchestrator.

[Näytön toimintoja](https://technet.microsoft.com/library/hh403827.aspx) ei muunneta sillä ei ole vastaava niihin Azure automaatio.  Poikkeus on [muunnettu integrointi paketteja](#integration-pack-converter) , jotka muunnetaan paikkamerkki-toimintoon näytön toimintoja.

Toiminto [, joka on muunnettu integrointi pack](#integration-pack-converter) -muuntuvat, jos integraatio moduulin polku **Moduulit** -parametrin.  System Center integrointi paketteja voit käyttää [System Center Orchestrator integrointi moduulit](#system-center-orchestrator-integration-modules).


### <a name="orchestrator-resources"></a>Orchestrator resurssit

Runbookin muuntimen muuntaa vain runbooks, ei esimerkiksi laskureita, muuttujien tai yhteydet muita Orchestrator resursseja.  Azure automaatio laskureita ei tueta.  Muuttujat ja yhteydet ovat tuettuja, mutta sinun on luotava ne manuaalisesti.  Lokitiedostojen ilmoittaa: n runbookin edellyttää näiden resurssien ja määritä vastaavan resurssit, jotka on luotava Azure automaatio toiminta muunnettu runbookin varten.

Runbookin voi käyttää esimerkiksi muuttujan täytä tehtävän tietyn arvon.  Muunnettu runbookin muuntaa tehtävän ja Määritä muuttujan resurssi Azure automaatio saman niminen Orchestrator muuttujana.  Tämä huomattava **Runbookin muuntimen - Summary.log** -tiedostossa, joka on luotu muuntamisen jälkeen.  Haluat luoda manuaalisesti muuttujan kohteen Azure automaatio ennen kuin käytät: n runbookin. 

 
### <a name="input-parameters"></a>Syöteparametrit

Runbooks Orchestrator Hyväksy syöteparametrit **Alusta tiedot** -toimintoa.  Jos muuntamisen runbookin sisältää tehtävän, tehtävä kunkin parametrin luodaan [syöteparametria](automation-graphical-authoring-intro.md#runbook-input-and-output) Azure automaatio-runbookin.  [Työnkulun komentosarjan ohjausobjektin](automation-graphical-authoring-intro.md#activities) tehtävä luodaan, joka hakee ja kunkin parametrin Palauttaa muunnetun runbookin.  Kaikki: n runbookin toiminnot, jotka käyttävät syöteparametria viitata tämän toiminnon tulos.

Tämä strategia käytetään johtuu peilikuvaksi parhaiten Orchestrator runbookin toimintoja.  Uusi graafinen runbooks toimintojen olisi viittaavat suoraan syötteen parametrit Runbookin syötteen tietolähteen avulla.


### <a name="invoke-runbook-activity"></a>Käynnistää Runbookin tehtävä

Runbooks Orchestrator Käynnistä muiden runbooks **Käynnistää Runbookin** -toimintoa. Jos muuntamisen runbookin sisältää tässä aktiviteetti ja **valmistumisen odottaminen** -asetus on määritetty, runbookin tehtävä luodaan sen-muunnettu runbookin.  Jos **valmistumisen odottaminen** -vaihtoehto ei ole määritetty, komentosarjan työnkulun tehtävä on luotu, joka käyttää **Käynnistä AzureAutomationRunbook** : n runbookin käynnistämiseen.  Kun tuot muunnettu runbookin Azure automaatio, sinun on muokattava tässä aktiviteetti tehtävän tiedot.




## <a name="related-articles"></a>Aiheeseen liittyviä artikkeleita

- [System Center 2012 - Orchestrator](http://technet.microsoft.com/library/hh237242.aspx)
- [Palvelun hallinta automatisointi](https://technet.microsoft.com/library/dn469260.aspx)
- [Hybrid Runbookin työntekijä](automation-hybrid-runbook-worker.md)
- [Orchestrator kuulumattomat](http://technet.microsoft.com/library/hh403832.aspx)
- [Lataa System Center Orchestrator siirron Työkalut](https://www.microsoft.com/en-us/download/details.aspx?id=47323)
 
