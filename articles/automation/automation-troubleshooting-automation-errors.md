<properties
   pageTitle="Azure automaatio Virheenkäsittely | Microsoft Azure"
   description="Tässä artikkelissa kuvataan basic virheen käsittely liittyviä ongelmia ja korjata Azure automaatio yleisiä virheitä."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="stevenka"
   editor="tysonn"
   tags="top-support-issue"
   keywords="automaatiovirhe, Virheenkäsittely"/>
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/06/2016"
   ms.author="sngun; v-reagie"/>

# <a name="error-handling-tips-for-common-azure-automation-errors"></a>Virheenkäsittely vihjeitä Azure automaatio Yleiset virheet

Tässä artikkelissa kerrotaan yleisiä Azure automaatio virheistä kärsiä, ja ehdottaa mahdollista Virheenkäsittely vaiheet.

## <a name="troubleshoot-authentication-errors-when-working-with-azure-automation-runbooks"></a>Vianmääritys, kun käsittelet Azure automaatio runbooks todennus  

### <a name="scenario-sign-in-to-azure-account-failed"></a>Skenaario: Kirjautuminen epäonnistui Azure-tilille

**Virhe:** Näyttöön tulee virhesanoma "Unknown_user_type: Tuntematon käyttäjätyyppi", kun käsittelet Lisää AzureAccount tai Kirjaudu sisään AzureRmAccount Cmdlet-komentoja.

**Virheen syy:** Tämä virhe ilmenee, jos tunnistetiedon resurssi nimi ei ole kelvollinen tai käyttäjänimi ja salasana, jota käytit automaatio tunnistetiedon kohteen asetukset eivät ole kelvollisia.

**Vianmääritysvihjeitä:** Jotta voit selvittää, miksei, tee seuraavat toimet:  

1. Varmista, että sinulla ei ole muita erikoismerkkejä, kuten **@** automaatio tunnistetiedon resurssi nimi, jonka avulla voit muodostaa yhteyden Azure merkki.  

2. Tarkista, että voit käyttää käyttäjänimi ja salasana, jotka on tallennettu Azure automaatio tunnistetieto paikallisen PowerShell ise:-editorissa. Voit tehdä tämän suorittamalla seuraavat cmdlet-komennot PowerShell ise::  

        $Cred = Get-Credential  
        #Using Azure Service Management   
        Add-AzureAccount –Credential $Cred  
        #Using Azure Resource Manager  
        Login-AzureRmAccount –Credential $Cred

3. Jos käyttöoikeuksien epäonnistuu paikallisesti, tämä tarkoittaa, että et ole vielä määrittänyt Azure Active Directory-tunnistetiedot oikein. Lisätietoja on blogikirjoituksessa [Authenticating Azure Azure Active Directoryn avulla](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) saat Azure Active Directory-tilin, joka on määritetty oikein.  


### <a name="scenario-unable-to-find-the-azure-subscription"></a>Skenaario: Ei löydy Azure tilaus

**Virhe:** Näyttöön tulee virhesanoma "-niminen tilauksen ``<subscription name>`` ei löydy" Valitse AzureSubscription tai valitse AzureRmSubscription Cmdlet-komentoja käytettäessä.

**Virheen syy:** Tämä virhe ilmenee, jos tilauksen nimi ei ole kelvollinen tai tilauksen järjestelmänvalvoja ei ole määritetty Azure Active Directory-käyttäjä, joka yrittää ottaa tilauksen tiedot.

**Vianmääritysvihjeitä:** Jotta voit selvittää, jos on oikein todennettu Azure ja käyttämään tilauksen, valitse rooliin, tee seuraavat toimet:  

1. Varmista, että suoritat **Lisää AzureAccount** , ennen kuin suoritat **Valitse AzureSubscription** cmdlet-komento.  

2. Jos näet edelleen virheilmoituksen, Muokkaa koodia lisäämällä seuraavan **Lisää AzureAccount** cmdlet-komento **Get-AzureSubscription** cmdlet-komento ja Suorita koodi.  Tarkista nyt, jos Get-AzureSubscription tulos sisältää tilauksen tiedot.  
    * Jos et näe kaikki tilauksen tiedot-tulosteen, tämä tarkoittaa, että tilaus ei ole vielä alustaa.  
    * Jos näet Tilaustiedot tulosteessa, varmista, että käytät oikeaa tilauksen nimi tai tunnus ja **Valitse AzureSubscription** cmdlet-komento.   


### <a name="scenario-authentication-to-azure-failed-because-multi-factor-authentication-is-enabled"></a>Skenaario: Azure-todennus epäonnistui, koska monimenetelmäisen todentamisen on otettu käyttöön

**Virhe:** Näyttöön tulee virhesanoma "Lisää AzureAccount: AADSTS50079: rekisteröinti vahva todennus (kuitti ylöspäin), jota tarvitaan" Kun todennuksen Azure Azure käyttäjänimellä ja salasanalla.

**Virheen syy:** Jos olet määrittänyt monimenetelmäisen todentamisen Azure-tili, et voi käyttää Azure Active Directory-käyttäjän todentaminen Azure.  Haluat käyttää varmenteen tai pääasiallista palvelu todentaa Azure.

**Vianmääritysvihjeitä:** Jos haluat käyttää varmenteen Azure hallinnan cmdlet-komennot, viitata [luominen ja lisääminen sertifikaatin Azure palveluiden hallinta.](http://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) Jos haluat käyttää palvelun lyhennys Azure resurssien hallinnan cmdlet-komennot, viittaavat [luominen palvelun lyhennys Azure-portaalissa](./resource-group-create-service-principal-portal.md) ja [todennustapa palvelun lyhennys Azure Resource Manageriin.](./resource-group-authenticate-service-principal.md)


## <a name="troubleshoot-common-errors-when-working-with-runbooks"></a>Vianmääritys yleisiä virheitä, kun käsittelet runbooks

### <a name="scenario-runbook-fails-because-of-deserialized-object"></a>Skenaario: Runbookin epäonnistuu, koska sarjoitus objekti

**Virhe:** Oman runbookin epäonnistuu virheen "ei voi sitoa parametrin ``<ParameterName>``. Ei voi muuntaa ``<ParameterType>`` tyypin Deserialized arvo ``<ParameterType>`` kirjoittaa ``<ParameterType>``".

**Virheen syy:** Jos yhteyttä runbookin PowerShell-työnkulun, tallennetaan monimutkaisia objekteja sarjoitus muodossa, jotta jatkuvat runbookin tila, jos työnkulku on hyllytetty.  

**Vianmääritysvihjeitä:**  
Jokin seuraavista kolmesta ratkaisuja korjaa tämän ongelman:

1. Jos ovat putkistot monimutkaisia objekteja yhden cmdlet-komennosta toiseen, Rivitä näiden cmdlet InlineScript.  
2. Välittää nimen tai arvon, joka on sen sijaan, että koko objektin kulkevan monimutkaisia objektista.  

3. Käytä PowerShell-runbookin PowerShell työnkulun runbookin sijaan.  


### <a name="scenario-runbook-job-failed-because-the-allocated-quota-exceeded"></a>Skenaario: Runbookin työ epäonnistui, koska varattuun kiintiö on ylitetty

**Virhe:** Runbookin-työ epäonnistuu virheen "kuukausittain yhteensä työn suoritusaika kiintiön on saavutettu tähän tilaukseen".

**Virheen syy:** Tämä virhe ilmenee, kun projektin suorittaminen ylittää 500 minuutin vapaa kiintiön tilissäsi. Tämän kiintiön koskee kaikenlaisia suorittamisen projektitehtävien kuten työn testaaminen, työn käynnistäminen-portaalista, työn suoritetaan käyttämällä webhooks ja ajoitetun työn suorittaminen käyttämällä joko Azure portaalin tai oman joten. Lisätietoja hinnat automatisointiin artikkelissa [automaatio hinnat](https://azure.microsoft.com/pricing/details/automation/).

**Vianmääritysvihjeitä:** Jos haluat käyttää yli 500 minuuttia kuukaudessa käsittelyn tarvitset tilauksen muuttamiseen vapaa taso Basic taso. Voit päivittää Basic taso noudattamalla seuraavia ohjeita:  

1. Azure-tilaukseen kirjautuminen  
2. Haluatko päivittää automaatio-tili  
3. Valitse **asetukset** > **hinnoittelu taso ja käyttö** > **hinnoittelu taso**  
4. Valitse **Valitse hinnoittelu taso** -sivu **Basic**    


### <a name="scenario-cmdlet-not-recognized-when-executing-a-runbook"></a>Skenaario: Cmdlet-komento ei tunnisteta suoritettaessa runbookin

**Virhe:** Runbookin-työ epäonnistuu virheen "``<cmdlet name>``: termi ``<cmdlet name>`` cmdlet-komento, funktion, komentosarjatiedosto tai käytössä ohjelman nimi ei tunnisteta."

**Virheen syy:** Tämä virhe aiheutuu, kun PowerShell-ohjelma ei löydä cmdlet-komento, käytössäsi on kohdassa runbookin.  Tämä voi johtua siitä cmdlet-komento, joka sisältää moduulia ei ole tiliä, on runbookin, nimellä nimiristiriita tai cmdlet on olemassa myös toisessa moduulissa ja automaatio ei voi selvittää nimi.

**Vianmääritysvihjeitä:** Jokin seuraavista toimista ratkaisee ongelman:  

- Tarkista, että cmdlet-nimi on kirjoitettu oikein.  

- Varmista, että cmdlet olemassa automaatio-tilisi ja että ristiriitoja ei. Voit tarkistaa, onko cmdlet-komento, Avaa runbookin muokkaustilasta valintatilaan ja Etsi cmdlet-komento, kirjaston tai suorita etsittävän **Get-komento ``<CommandName>`` **.  Kun on vahvistettu, cmdlet-komento on käytettävissä tiliä ja että nimiristiriitoja ei ole muissa cmdlet-komennot tai runbooks, lisää se alusta ja varmistaa, että käytössäsi on kelvollinen parametri määrittää oman runbookin.  

- Jos sinulla on nimiristiriita ja cmdlet-komento on käytettävissä kaksi eri moduuleissa, voit korjata tämän käyttämällä täydellistä nimi-cmdlet-komennolla. Voit esimerkiksi **ModuleName\CmdletName**.  

- Jos suoritetaan runbookin paikallisen hybrid työntekijä ryhmän, varmista, että moduulin/cmdlet-komento on asennettu hybrid työntekijä isännöivän tietokoneen.


### <a name="scenario-a-long-running-runbook-consistently-fails-with-the-exception-the-job-cannot-continue-running-because-it-was-repeatedly-evicted-from-the-same-checkpoint"></a>Skenaario: Pitkään käynnissä runbookin johdonmukaisesti epäonnistuu lukuun ottamatta: "työ ei voi jatkaa käynnissä, koska se on toistuvasti poistaa saman tarkistuspiste".

**Virheen syy:** Tämä on rakenne toiminnan vuoksi Azure automaatio, joka runbookin keskeyttää automaattisesti, jos suorittaa yli 3 tuntia prosesseja "Osuus" seuranta. Kuitenkin palauttaa virhesanoman ei tarjoa "mitä seuraavaksi" asetukset. Runbookin voidaan siirtää useista eri syistä. Keskeyttää ilmaantui enimmäkseen virheiden vuoksi. Esimerkiksi poikkeus runbookin, verkkovirhe tai kaatumisen käynnissä: n runbookin Runbookin työntekijän, kaikki aiheuttavat runbookin keskeytetään ja Aloita edellisen tarkistuspisteen kun sen käyttöä jatketaan.

**Vianmääritysvihjeitä:** Voit välttää tämän ongelman kuvata ratkaisu on käyttää tarkistuspisteet työnkulussa.  Lisätietoja Lisää viitata [Learning PowerShell työnkulkuja](automation-powershell-workflow.md#Checkpoints).  Yksityiskohtaisemmin "Osuus" ja tarkistuspiste löytyy blogin tämän artikkelin [Avulla tarkistuspisteet-Runbooks](https://azure.microsoft.com/en-us/blog/azure-automation-reliable-fault-tolerant-runbook-execution-using-checkpoints/).


## <a name="troubleshoot-common-errors-when-importing-modules"></a>Vianmääritys yleisiä virheitä, kun tuot moduulit

### <a name="scenario-module-fails-to-import-or-cmdlets-cant-be-executed-after-importing"></a>Skenaario: Moduuli epäonnistuu, voit tuoda tai Cmdlet-komentoja ei voi suorittaa, kun olet tuonut

**Virhe:** Moduulin epäonnistuu, jos haluat tuoda tai tuonti onnistuu, mutta ei cmdlet-komennot puretaan.

**Virheen syy:** Joka moduuli ehkä ei onnistunut tuoda Azure automaatio yleisimmät syyt ovat seuraavat:  

- Rakenne ei vastaa automaatio on olevan rakenne.  

- Moduulin on riippuvainen toisen moduuli, jolla ei ole otettu automaatio-tiliisi.  

- Moduulin puuttuu riippuvaa-kansiossa.  

- **Uusi AzureRmAutomationModule** cmdlet-komento on käytössä moduuli ladataan ja annetut ei tallennustilan koko polku tai ei ole ladattu moduulin Julkinen URL-Osoitteen kautta.  

**Vianmääritysvihjeitä:**  
Jokin seuraavista toimista ratkaisee ongelman:  

- Varmista, että moduulin seuraa seuraavanlainen:  
ModuleName.Zip **->** ModuleName tai versionumero **->** (ModuleName.psm1, ModuleName.psd1)

- Avaa .psd1-tiedosto ja onko moduulin riippuvuuksia.  Jos näin on, lataa näitä moduuleja automaatio-tilille.  

- Varmista, että kaikki viitatun .dll löytyvät moduuli-kansiossa.  


## <a name="troubleshoot-common-errors-when-working-with-desired-state-configuration-dsc"></a>Yleisten virheiden vianmääritys työskenneltäessä kanssa toivottuja tilan määrittäminen (DSC)  

### <a name="scenario-node-is-in-failed-status-with-a-not-found-error"></a>Skenaario: Solmu on epäonnistunut tila, kun "Ei löydy"-Virhe

**Virhe:** Solmun on **epäonnistunut** tila ja virheen sisältävä raportti "yrittää saada toiminnon palvelimen https://``<url>``//accounts/``<account-id>``/Nodes(AgentId=``<agent-id>``)/GetDscAction epäonnistui, koska kelvollinen määritysten ``<guid>`` ei löydy."

**Virheen syy:** Tämä virhe ilmenee yleensä kun solmu on määritetty määrityksen nimen (esimerkiksi ABC) sen sijaan, että solmu määrityksen nimen (esimerkiksi ABC. WebServer).  

**Vianmääritysvihjeitä:**  

- Varmista, että määrität solmu ja "solmu määritysten nimi" ei "konfiguroinnin nimi".  

- Voit määrittää solmun kokoonpanossa Azure-portaalissa solmu tai PowerShell-cmdlet-komennon.
    - Jotta voit määrittää solmun kokoonpanossa solmu Azure-portaalissa, Avaa **DSC solmut** -sivu ja valitse Valitse solmu ja valitse **solmu kokoonpanon määrittäminen** -painiketta.  
    - Jotta voit määrittää solmun kokoonpanossa PowerShell-cmdlet-komennolla solmu, **Määritä AzureRmAutomationDscNode** cmdlet-komennolla


### <a name="scenario--no-node-configurations-mof-files-were-produced-when-a-configuration-is-compiled"></a>Skenaario: Ei solmu-määrityksiä (MOF-tiedostot) on valmistettu, kun määrityksen käännetään

**Virhe:** DSC kääntäminen työtäsi keskeyttää virheen: "kääntäminen onnistui, mutta ei solmu määritysten .mofs luotiin".

**Virheen syy:** Kun valmistettu lausekkeen seuraavat DSC määrityksessä **solmu** -avainsanan arvioi $null sitten solmu ei ole määrityksiä.    

**Vianmääritysvihjeitä:**  
Jokin seuraavista toimista ratkaisee ongelman:  

- Varmista, että lausekkeen vieressä kokoonpanon määrityksessä **solmu** -avainsana on ei arvioimisen $null.  
- Jos ovat kulkeva ConfigurationData muodostettaessa määritystä, varmista, että voit ovat kulkeva odotettu arvot, jotka määritykset edellyttää [ConfigurationData](automation-dsc-compile.md#configurationdata).


### <a name="scenario--the-dsc-node-report-becomes-stuck-in-progress-state"></a>Skenaario: DSC solmu raportti tulee jumissa "käynnissä" tila

**Virhe:** DSC agentti tulostaa "Ei ole annettu ominaisuusarvoihin havaitsi esiintymä."

**Virheen syy:** Olet päivittänyt WMF-versio ja WMI on vioittunut.  

**Vianmääritysvihjeitä:** Korjaa [DSC tunnettuja ongelmia ja rajoituksia](https://msdn.microsoft.com/powershell/wmf/limitation_dsc) blogimerkinnän ohjeiden mukaan.


### <a name="scenario--unable-to-use-a-credential-in-a-dsc-configuration"></a>Skenaario: Ei voi käyttää olevan tunnistetiedon DSC-määritys

**Virhe:** DSC kääntäminen työ on keskeytetty virheen: "System.InvalidOperationException virhe käsiteltäessä ominaisuuden tunnistetiedon, jos tyypin ``<some resource name>``: muuntaminen ja tallentaminen salattu salasana tekstimuodossa on sallittu vain, jos PSDscAllowPlainTextPassword on määritetty tosi".

**Virheen syy:** Kehitetty olevan tunnistetiedon määrityksen mutta et voi ERISNIMI **ConfigurationData** **PSDscAllowPlainTextPassword** arvoksi TOSI, jos kunkin solmu-määritys.  

**Vianmääritysvihjeitä:**  
- Varmista, että oikea **ConfigurationData** **PSDscAllowPlainTextPassword** arvoksi true kunkin solmu kokoonpanon määrityksessä mainituista Välitä. Saat lisätietoja viitata [Azure automaatio DSC kohteita](automation-dsc-compile.md#assets).


## <a name="next-steps"></a>Seuraavat vaiheet

Jos olet noudattanut edellä vianmääritysohjeita kaikkia ja tarvitset lisäohjeita milloin tahansa tämän artikkelin, voit:

- Pyydä apua Azure asiantuntijoilta. Ongelmaa, Lähetä [MSDN Azure tai Pinon ylivuoto keskustelupalstoilla.](https://azure.microsoft.com/support/forums/)

- Tiedoston Azure tuki tapahtuma. Siirry [Azure tukevat sivusto](https://azure.microsoft.com/support/options/) ja valitse **teknistä tai laskutukseen liittyvää**tukea **tuesta** .

- Kirjaa komentosarja Request- [Komentosarjat](https://azure.microsoft.com/documentation/scripts/) , jos etsit Azure automaatio-runbookin ratkaisu tai integrointi-moduuli.

- Kirjaa palautetta tai ominaisuus pyynnöt Azure automaatio [Käyttäjän ääni](https://feedback.azure.com/forums/34192--general-feedback).
