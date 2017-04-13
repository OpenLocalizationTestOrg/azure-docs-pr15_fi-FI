<properties 
pageTitle="Suorita käynnistys tehtävät Azure pilvipalveluihin | Microsoft Azure" 
description="Käynnistyksen tehtävien avulla, kun sovellus pilven palvelun lisätietoja ympäristön valmistelemisesta. Tämä videokurssi sisältää käynnistys tehtävät maksaminen ja kuinka voit tehdä muutoksia" 
services="cloud-services" 
documentationCenter="" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>



# <a name="how-to-configure-and-run-startup-tasks-for-a-cloud-service"></a>Määrittäminen ja suorita käynnistys tehtävistä pilvipalveluun

Käynnistyksen tehtävien avulla voit suorittaa toimintoja, ennen kuin roolin käynnistyy. Toiminnoista, joita haluat ehkä suorittaa ole komponentti asennetaan, rekisteröiminen COM-komponentteja, asetus rekisteriavaimet tai pitkään käynnissä prosessi käynnistetään.

>[AZURE.NOTE] Käynnistyksen tehtävät eivät ole käytettävissä näennäiskoneiden, vain Cloud Service-Web- ja työntekijä roolit.

## <a name="how-startup-tasks-work"></a>Käynnistyksen tehtävät maksaminen

Käynnistyksen tehtävät ovat toimenpiteet, jotka ovat ennen oman roolit alkamis- ja on määritetty [ServiceDefinition.csdef] -tiedostossa käyttämällä [tehtävän] osan [Käynnistys] -elementissä. Usein käynnistys tehtävät ovat erä tiedostoja, mutta niitä voi olla myös konsolin sovellusten tai erä-tiedostoja, jotka alkavat PowerShell-komentosarjojen.

Ympäristömuuttujat välittää tietoja käynnistys tehtävään ja paikallinen tallennus voidaan välittämiseen ulos käynnistys-tehtävän. Esimerkiksi ympäristömuuttuja voit määrittää ohjelman, jonka aiot asentaa polku ja tiedostoja voidaan kirjoittaa paikalliseen tallennussijaintiin, joka voidaan lukea myöhemmin sitten käyttäjän roolia.

Käynnistyksen tehtävän voi kirjautua määrittämän kansion **TEMP** -ympäristömuuttuja tiedot ja virheet. Tehtävän käynnistyksen aikana **TEMP** -ympäristömuuttuja ratkaisee *C:\\resurssien\\temp\\[guid]. [ rolename]\\RoleTemp* directory pilveen käytettäessä.

Käynnistyksen tehtävät myös voidaan suorittaa useita kertoja välillä käynnistyy. Esimerkiksi käynnistys-tehtävä suoritetaan aina, kun rooli kierrättää ja roolin kierrättää ei välttämättä aina sisällä uudelleen. Käynnistyksen tehtävät on kirjoitettava tavalla, jonka avulla voit suorittaa useita kertoja ongelmitta.

Käynnistyksen tehtävät lopussa on oltava **komennon** (tai lopetuskoodi) nolla käynnistyksen suorittamiseen. Jos käynnistys tehtävän päättyy ei ole nolla- **komennon**, rooli ei käynnisty.


## <a name="role-startup-order"></a>Roolien käynnistys järjestys

Seuraavassa on luettelo Azure roolin käynnistys-toimintosarjan:

1. Esiintymää on merkitty **Aloitus** ja vastaanota liikenne.

2. Kaikki käynnistys tehtävät suoritetaan **taskType** niiden määritteiden perusteella.
    - **Yksinkertaiset** tehtävät suoritetaan synkronoidusti, yksi kerrallaan.
    - **Taustan** ja **Edustan** tehtävät ovat Aloita asynkronisesti, yhdensuuntaisesti käynnistys-tehtävän.  
       
    > [AZURE.WARNING] IIS ei ole täysin määritetty käynnistys tehtävän vaiheessa käynnistysprosessin mukaisesti siten roolin tiedot eivät välttämättä ole käytettävissä. Käynnistyksen tehtäviä, joihin tarvitaan rooli-tietojen käytettävä [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx).

3. Rooli host-prosessi on käynnissä ja sivusto on luotu IIS.

4. [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) -menetelmää kutsutaan.

5. Esiintymää on merkitty **valmiiksi** ja liikenne reititetään esiintymä.

6. [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) -menetelmää kutsutaan.


## <a name="example-of-a-startup-task"></a>Esimerkki käynnistys tehtävän

Käynnistyksen tehtävät määritetään [ServiceDefinition.csdef] tiedoston, **tehtävä** -osaan. **Komentorivi** -määrite määrittää nimen ja parametrit käynnistyksen komentojonotiedosto console-komento, **executionContext** -määrite määrittää käynnistys-tehtävän käyttöoikeustaso ja **taskType** -määrite määrittää, miten tehtävän suoritetaan.

Tässä esimerkissä ympäristömuuttuja **MyVersionNumber**käynnistys tehtävän luodaan ja määritetty arvo "**1.0.0.0**".

**ServiceDefinition.csdef**:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
        <Environment>
            <Variable name="MyVersionNumber" value="1.0.0.0" />
        </Environment>
    </Task>
</Startup>
```

Seuraavassa esimerkissä **Startup.cmd** komentojonotiedosto kirjoittaa rivin "Nykyinen versio on 1.0.0.0" StartupLog.txt tiedoston TEMP-ympäristömuuttuja määrittämää hakemistossa. `EXIT /B 0` Viiva, joka kertoo käynnistys tehtävän päättyy **komennon** nolla.

```cmd
ECHO The current version is %MyVersionNumber% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT /B 0
```

> [AZURE.NOTE] Visual Studiossa käynnistys komentojonotiedosto **Kopioi tulosteen kansioon** -ominaisuuden on asetettava **Aina kopioiminen** toiseen ja varmistamalla, että käynnistys komentojonotiedosto on otettu asianmukaisesti käyttöön projektiin Azure-(**approot\\bin** Web roolit ja **approot** työntekijä roolien).

## <a name="description-of-task-attributes"></a>Kuvaus tehtävän määritteet

Seuraavassa kuvataan **tehtävän** osan [ServiceDefinition.csdef] tiedoston määritteet:

**Komentorivi** - määrittää komentorivin käynnistys-tehtävän.

- Komento-valinnainen komentoriviltä parametreilla, joka alkaa käynnistys-tehtävän.
- Tämä on usein cmd. tai .bat komentojonotiedosto tiedostonimi.
- Tehtävä on suhteessa AppRoot\\käyttöönoton Bin-kansio. Ympäristömuuttujat laajennetaan ei polku ja tiedostonimi tehtävän määrittämiseen. Jos ympäristössä laajentamista vaaditaan, voit luoda pieni cmd. komentosarjan, joka kutsuu käynnistys-tehtävän.
- Voi olla console-sovelluksen tai komentojonotiedosto, joka alkaa [PowerShell-komentosarjaa](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task).

**executionContext** - määrittää käynnistys tehtävän käyttöoikeustaso. Käyttöoikeustaso voidaan rajoittaa tai laajennetut:

- **rajoitettu**  
Käynnistyksen tehtävää suoritetaan samat oikeudet kuin rooli. Kun [Runtime] -elementin **executionContext** -määrite on **rajoitettu**, käyttöoikeuksia käytetään.

- **laajennettuja**  
Käynnistys suoritetaan järjestelmänvalvojan oikeuksilla. Näin käynnistys tehtäviä asentaa ohjelmia, tekemällä IIS määritysmuutoksia, suorittaa rekisterimuutokset ja muut järjestelmänvalvojan tason tehtävät-roolin itse käyttöoikeustaso määrä kasvaa.  

> [AZURE.NOTE] Käyttöoikeustaso käynnistys tehtävän ei tarvitse olla sama kuin itse rooli.

**taskType** - määrittää, miten käynnistys-tehtävä on suoritettu.

- **Yksinkertainen**  
Tehtävät suoritetaan synkronoidusti, yksi kerrallaan järjestyksessä määritetty [ServiceDefinition.csdef] -tiedostossa. Kun yksi **Yksinkertainen** käynnistys-tehtävä loppuu **komennon** nolla-seuraava **Yksinkertainen** käynnistys tehtävä on suoritettu. Jos määritettynä on ei lisää **yksinkertaisten** käynnistys tehtävien suorittamiseen, roolin itse käynnistetään.   

    > [AZURE.NOTE] Jos **yksinkertaisen** tehtävien päättyy ei ole nolla- **komennon**, esiintymän estetään. Seuraavien **yksinkertaisten** käynnistys tehtävät ja itse rooli ei käynnisty.

    Voit varmistaa, että komentojonotiedosto päättyy **komennon** nolla, suorita komento `EXIT /B 0` tiedoston eräkäsittely lopussa.

- **tausta**  
Tehtävät suoritetaan asynkronisesti roolin käynnistykseen rinnakkain.

- **edustan**  
Tehtävät suoritetaan asynkronisesti roolin käynnistykseen rinnakkain. **Edustan** ja **taustan** tehtävän avaimen ero on **edustalla** tehtävän estää rooli kierrättäminen tai suljetaan ennen kuin tehtävä on päättynyt. **Taustatehtävät** ei ole tämä rajoitus.

## <a name="environment-variables"></a>Ympäristömuuttujat

Ympäristön muuttujia voi välittää tietoja käynnistys-tehtävän. Voit esimerkiksi sijoittaa polkua, joka sisältää ohjelman, voit asentaa-Blob-objektien porttinumerot, joka käyttää tehtäväsi tai hallintaominaisuudet käynnistys tehtävän asetukset.

Ympäristön muuttujien käynnistys tehtävien; kahdenlaisia staattinen ympäristömuuttujat ja ympäristömuuttujat jäsenet [RoleEnvironment] luokan perusteella. Molemmat ovat [ServiceDefinition.csdef] tiedoston [ympäristön] -kohtaan ja käyttää sekä [muuttujan] elementin ja **nimi** -määrite.

Staattinen ympäristön muuttujaa käytetään [muuttujan] elementin **value** -määrite. Edellä oleva esimerkkilauseke Luo ympäristömuuttuja **MyVersionNumber** , jossa on staattinen arvo on "**1.0.0.0**". Toinen esimerkki olisi luominen **StagingOrProduction** ympäristömuuttuja joka voidaan määrittää arvot "**väliaikaisen**" tai "**tuotannon**" manuaalisesti suorittamaan eri käynnistys toiminnot **StagingOrProduction** ympäristömuuttuja arvon perusteella.

Ympäristömuuttujat jäsenet RoleEnvironment luokan perusteella Älä käytä [muuttujan] elementin **value** -määrite. Sen sijaan [RoleInstanceValue] -alielementti sopivan **XPath** määritteen arvon, voidaan luoda ympäristömuuttuja tietyn jäsenen [RoleEnvironment] luokan perusteella. Voit käyttää eri [RoleEnvironment] arvojen **XPath** -määritteen arvot [tähän](cloud-services-role-config-xpath.md).



Luodaksesi ympäristömuuttuja, joka on "**true**", kun esiintymä toimii Laske emulaattorin ja "**false**", kun käynnissä pilvipalvelussa, käytä [muuttujan] ja [RoleInstanceValue] seuraavat osat:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
    
            <!-- Create the environment variable that informs the startup task whether it is running
                in the Compute Emulator or in the cloud. "%ComputeEmulatorRunning%"=="true" when
                running in the Compute Emulator, "%ComputeEmulatorRunning%"=="false" when running
                in the cloud. -->
    
            <Variable name="ComputeEmulatorRunning">
                <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
            </Variable>
    
        </Environment>
    </Task>
</Startup>
```

## <a name="next-steps"></a>Seuraavat vaiheet
Lue, miten suorittaa joitakin [käynnistämisen perustoiminnot](cloud-services-startup-tasks-common.md) oman pilvipalvelussa.

[Paketin](cloud-services-model-and-package.md) pilvipalvelussa sijaitsevaan.  


[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[Tehtävä]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Käynnistys]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Runtime]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[Ympäristön]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[Muuttujan]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx