<properties
    pageTitle="Mikä on Azure automaatio | Microsoft Azure"
    description="Katso, mikä arvo Azure automaatio tarjoaa ja vastauksia yleisiin kysymyksiin, jotka niin, että voit aloittaa luomisessa, runbooks ja Azure automaatio DSC avulla."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""
    keywords="Mikä on automaatio, azure automaatio ja azure automaatio esimerkkejä"/>
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article" 
    ms.date="05/10/2016"
    ms.author="magoedte;bwren"/>

# <a name="azure-automation-overview"></a>Azure automaatio-yleiskatsaus

Microsoft Azure automaatio avulla käyttäjät voivat automatisoida manuaalinen, pitkään suoritettavien virhe voi enää tai usein toistuvia tehtäviä, jotka suoritetaan usein cloud- ja enterprise-ympäristössä. Se säästää aikaa ja lisää säännöllisesti hallintatehtäviä luotettavuutta ja ajoittaa myös ne suoritetaan automaattisesti säännöllisin väliajoin. Voit automatisoida runbooks käytön prosessien tai automatisoida määritystenhallintaa käyttämällä toivottuja tilan määritys. Tässä artikkelissa on lyhyt yleiskatsaus Azure automaatio ja vastauksia yleisiin kysymyksiin. Voit viitata toisiin ohjeaiheisiin, tämän kirjaston tarkempia tietoja eri aiheista.


## <a name="automating-processes-with-runbooks"></a>Prosessien havainnollistaminen runbooks automatisointi

Runbookin on tehtäväjoukko, joka suorittaa joitakin automatisoidun prosessin Azure automaatio. Voi olla esimerkiksi virtual machine alkamis- ja luomisesta lokitapahtuman yksinkertaista tai joudut ehkä monimutkaisia runbookin, joka yhdistää muiden pienempi runbooks suoritettava monimutkainen prosessi useita resursseja tai jopa usean paveikslėlis yli ja paikalliseen ympäristössä.  

Esimerkiksi voi on aiemmin manuaalinen prosessi, jossa katkaistaan SQL-tietokantaan, jos se on lähellä enimmäiskoon, joka sisältää useita vaiheita muodostetaan yhteys palvelimeen, kuten tietokantayhteyden muodostamisessa, saat tietokannan koon, tarkista kynnysarvo ylittänyt ja sitten Katkaise sen ja lähettää ilmoituksen käyttäjälle. Sen sijaan, että manuaalisesti suorittamiseen kunkin seuraavasti, voit luoda runbookin, jotka suorittaa kaikki tehtävät yksittäisen prosessin. Aloittaa: n runbookin, anna tarvittavat tiedot, kuten SQL-palvelimen nimen, tietokannan nimi ja vastaanottajien sähköposti ja istut sitten takaisin, kun prosessi on valmis. 


## <a name="what-can-runbooks-automate"></a>Mitä runbooks voit automatisoida?

Azure automaatio Runbooks perustuvat Windows PowerShell- tai Windows PowerShell-työnkulun, niin he tehdä mitään PowerShell tehtävät. Jos sovelluksen tai palvelun Ohjelmointirajapinta, valitse runbookin voivat käyttää sitä. Jos sinulla on PowerShell-moduulin sovelluksen, voit ladata moduulin Azure automaatio ja Sisällytä näiden cmdlet-komennot oman runbookin. Azure automaatio runbooks suorittaa Azure pilveen, ja voit käyttää minkä tahansa cloud resurssit tai Ulkoiset resurssit, joita voidaan käyttää pilvestä. Käytä [Hybrid Runbookin työntekijä](automation-hybrid-runbook-worker.md), runbooks suorittaa paikallisen tietokeskuksen hallittavan paikalliset resurssit. 


## <a name="getting-runbooks-from-the-community"></a>Käytön runbooks yhteisöstä

[Runbookin valikoima](automation-runbook-gallery.md#runbooks-in-runbook-gallery) on Microsoftin runbooks ja voit käyttää yhteisön ympäristössäsi muuttumattomina tai mukauttaa niitä omia tarkoituksiin. He ovat myös hyötyä Lue, miten voit luoda oman runbooks viittauksina. Voit myös edistää oman runbooks, jolla muut käyttäjät voivat hyödyllisiä valikoimaan. 


## <a name="creating-runbooks-with-azure-automation"></a>Luomalla Runbooks Azure automaatio 

Voit [luoda oman runbooks](automation-creating-importing-runbook.md) alusta alkaen tai muokata runbooks omien tarpeidesi [Runbookin valikoima](http://msdn.microsoft.com/library/azure/dn781422.aspx) . Jakautuvat kolmeen eri [runbookin tyypit](automation-runbook-types.md) , voit valita vaatimukset ja PowerShell kokemus perusteella. Jos haluat käsitellä PowerShell-koodi, valitse voit [PowerShell runbookin](automation-runbook-types.md#powershell-runbooks) tai [PowerShell työnkulun runbookin](automation-runbook-types.md#powershell-workflow-runbooks) , voit muokata offline-tilassa tai [tekstimuotoinen editorin](http://msdn.microsoft.com/library/azure/dn879137.aspx) Azure-portaalissa. Jos haluat muokata runbookin ilman pohjana olevan koodin julkaisemisen, voit luoda [Graafinen runbookin](automation-runbook-types.md#graphical-runbooks) [Graafinen editorin](automation-graphical-authoring-intro.md) käyttäminen Azure-portaalissa. 

Mieluummin katsominen lukeminen? Osoitteesta Microsoft Ignite istunnosta toukokuu 2015 videon alapuolella. Huomautus: Vaikka käsitteitä ja tässä videossa kuvatut ominaisuudet ovat oikein, Azure automaatio on edennyt usein, koska tämä video on tallennettu, se nyt on laajemmat Käyttöliittymä Azure-portaalissa ja tukee lisäominaisuuksia.

> [AZURE.VIDEO microsoft-ignite-2015-automating-operational-and-management-tasks-using-azure-automation]


## <a name="automating-configuration-management-with-desired-state-configuration"></a>Haluttu tilan määritys automatisointi määritysten hallinta 

[PowerShellin DSC](https://technet.microsoft.com/library/dn249912.aspx) on hallinta-ympäristö, jonka avulla voit hallita, ottaminen käyttöön ja Pakota fyysinen isäntien ja syntaksin määritettäviä PowerShell näennäiskoneiden määritykset. Voit määrittää palvelimessa keskitetyn DSC erotettu, kohde koneet voit hakea ja ottaa määrityksiä. DSC on PowerShellin cmdlet-komennot, joiden avulla voit hallita määrityksiä ja resurssit.  

[Azure automaatio DSC](automation-dsc-overview.md) on pilvipohjainen ratkaisu, joka tarjoaa services vaaditaan yritysympäristössä PowerShell DSC.  Voit hallita DSC resursseja Azure automaatio- ja käytä käyttömahdollisuudet virtual tai fyysinen koneet, joka noutaa Azure pilveen DSC erotettu palvelimeen.  Se sisältää myös reporting services, joka ilmoittaa sinulle tärkeitä tapahtumia, kuten solmujen olet tarkistanut niiden varattujen asetuksista, kun uusi kokoonpano on otettu käyttöön. 


## <a name="creating-your-own-dsc-configurations-with-azure-automation"></a>Luomalla Azure automaatio DSC Omat määritykset

[DSC käyttömahdollisuudet](automation-dsc-overview.md#azure-automation-dsc-terms) Määritä solmu haluttu tila.  Useita solmujen käyttää määrityksen mukaisesti varmistaaksesi, että ne kaikki samat tilan ylläpitoa.  Voit luoda tekstiä käyttämällä määrityksen editorin paikallisessa tietokoneessa ja tuomalla ne Azure automaatio, johon voit kääntää sitä ja käyttää sitä solmujen.


## <a name="getting-modules-and-configurations"></a>Käytön moduulit ja määritykset 

Saat [PowerShell moduulit](automation-runbook-gallery.md#modules-in-powershell-gallery) sisältävä cmdlet-komennot, joita voit käyttää runbooks ja DSC käyttömahdollisuudet [PowerShell-valikoima](http://www.powershellgallery.com/). Voit käynnistää valikoiman Azure-portaalista ja tuoda moduulit suoraan Azure automaatio tai voit ladata ja tuoda ne manuaalisesti. Voit ladata ne, mutta ei voi asentaa moduulit suoraan Azure portaalin asentaa ne aivan kuten muita moduuli. 


## <a name="example-practical-applications-of-azure-automation"></a>Esimerkki Azure automaatio käytännön sovellukset 

Seuraavassa on muutamia esimerkkejä, mitä mahdollisiin automaatio Azure automaatio kanssa. 

* Luo ja kopioi näennäiskoneiden eri Azure-tilauksissa. 
* Aikataulutiedoston kopioi Paikallinen kone Azure-Blob-säiliö säilöön. 
* Automatisoida suojaustoiminnot, kuten estä pyytää asiakaskoneesta havaitessaan joka eston. 
* Varmista koneet tasassa jatkuvasti määritetyn suojauskäytäntö.
* Jatkuva käyttöönoton sovelluksen koodin hallinta cloud ja paikallisen infrastruktuuria. 
* Muodosta Active Directory metsää Azure, kurssin-ympäristössä. 
* Katkaise taulukon SQL-tietokantaan, jos DB on lähellä enimmäiskoon. 
* Päivitä etäyhteyden ympäristöasetuksia Azure-sivustoon. 


## <a name="how-does-azure-automation-relate-to-other-automation-tools"></a>Kuinka Azure automaatio liittyvät automaatio-työkaluja?

[Service Management automaatio (SMA)](http://technet.microsoft.com/library/dn469260.aspx) on tarkoitus hallintatehtävien automatisointiin yksityinen pilveen. Se on asennettu paikallisesti data Centerissä [Microsoft Azure Pack](https://www.microsoft.com/en-us/server-cloud/)osana. SMA ja Azure automaatio Käytä Windows PowerShell- ja Windows PowerShellin työnkulun perusteella saman runbookin muotoilun, mutta SMA ei tue [Graafinen runbooks](automation-graphical-authoring-intro.md).  

[System Center 2012 Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) on tarkoitettu automaatio paikallisen resursseja. Se käyttää eri runbookin muodossa kuin Azure automaatio ja Service Management automaatio ja luo runbooks niin, ettei mitään komentosarjan graafisessa käyttöliittymässä. Sen runbooks muodostuvat toiminta integrointi-paketteja, jotka on kirjoitettu Orchestrator varten. 


## <a name="where-can-i-get-more-information"></a>Mistä saan lisätietoja? 

Lisätietoja Azure automaatio ja luoda omia runbooks käytettävissä olevat eri resurssien. 

* **Azure automaatio kirjasto** on kohtaa, johon olet juuri nyt. Tämä kirjasto on artikkeleissa Anna täydelliset asiakirjat määrityksen ja hallinnan Azure automaatio ja käyttäjän Omat runbooks. 
* [Azure PowerShellin cmdlet-komennot](http://msdn.microsoft.com/library/jj156055.aspx) ohjeita automatisointi Windows PowerShellin Azure toimintoja. Runbooks Azure resurssien käsitteleminen näiden cmdlet-komentojen avulla. 
* [Hallinta-blogissa](https://azure.microsoft.com/blog/tag/azure-automation/) on uusimmat tiedot Microsoftin Azure automaatio ja muita tekniikoita hallinta. Tilaa tämä blogiin pysyt ajan tasalla Azure automaatio-ryhmän päivään. 
* [Automaatio-keskustelupalstalla,](http://go.microsoft.com/fwlink/p/?LinkId=390561) voit lähettää kysymyksiä Azure automaatio pystyttävä täyttämään Microsoftin ja automaatio-yhteisöön. 
* [Azure automaatio cmdlet-komennot](https://msdn.microsoft.com/library/mt244122.aspx) ohjeita hallintatehtävien automatisointiin. Se sisältää cmdlet-komentojen automaatio-tilejä, resurssien, runbooks, DSC.


## <a name="can-i-provide-feedback"></a>Voit antaa palautetta? 

**Anna meille palautetta.** Jos etsit Azure automaatio-runbookin ratkaisu tai integrointi-moduulin kirjaa komentosarjat komentosarjan pyytää. Jos sinulla palautetta tai ominaisuus pyynnöt Azure automatisointiin, julkaise ne käyttöön [Käyttäjän ääni](http://feedback.windowsazure.com/forums/34192--general-feedback). Kiitos! 


