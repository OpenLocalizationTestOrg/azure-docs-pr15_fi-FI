<properties 
   pageTitle="Lapsen runbooks Azure automaatio | Microsoft Azure"
   description="Tässä artikkelissa kuvataan lähtien runbookin Azure automaatio-toiseen runbookin ja niiden välillä tietojen jakaminen muilla tavoilla."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/17/2016"
   ms.author="magoedte;bwren" />

# <a name="child-runbooks-in-azure-automation"></a>Lapsen runbooks Azure automaatio-

Azure automaatio kirjoittaa Uudelleenkäytettävä, moduulit runbooks erillinen-funktiolla, joka voidaan käyttää muita runbooks parhaista käytännöistä on. Ylemmän tason runbookin usein soittaa yhden tai useamman lapsen runbooks suorittamiseen tarvittavat toiminnot. Soita lapsen runbookin kahdella tavalla ja jokaisella on selviä eroja täytyy ymmärtää niin, että voit määrittää, joka on paras eri skenaarioissa.

##  <a name="invoking-a-child-runbook-using-inline-execution"></a>Ali-runbookin käyttämällä sisäisiä suorittamisen käynnistettäessä

Käynnistää runbookin-riveillä kohteesta toiseen runbookin: n runbookin nimeä ja anna sen parametrien arvot tarkalleen siinä muodossa, kuten vakiomittoja aktiviteetin tai cmdlet-komento.  Kaikki runbooks automaatio saman tilin ovat käytettävissä muut käytettävän tällä tavalla. Ylemmän tason runbookin odottaa lapsen runbookin suorittamiseen ennen siirtämistä seuraavalle riville ja tuloksen palauttaa suoraan ylemmän tason.

Kun suoritat runbookin tekstiin, se suoritetaan samalla projektin nimellä ylemmän tason runbookin. On ei Työhistoria maininta lapsen runbookin, se suoritettiin. Poikkeukset- ja minkä tahansa stream tulosteen lapsen runbookin on liitetty ylemmän tason. Tuloksena on vähemmän töitä, ja helpottaa niiden löytämistä, voit seurata ja vianmääritys, koska päätason työ liittyvät poikkeukset-virhe ilmenee mukaan lapsen runbookin ja stream tulos.

Kun runbookin julkaistaan, minkä tahansa lapsen runbooks, joka kutsuu on jo julkaistu. Tämä johtuu siitä Azure automaatio muodostaa tiedonsiirtosuhteen minkä tahansa lapsen runbooks kanssa, kun runbookin käännetään. Jos ne eivät ole Ylemmän tason runbookin näkyvät julkaista oikein, mutta luo poikkeuksen, kun se käynnistetään. Jos näin käy, voit julkaista ylemmän tason runbookin haluat viitata oikein lapsen runbooks. Sinun ei tarvitse julkaista ylemmän tason runbookin, jos jokin lapsen runbooks on muutettu, koska suhde jo luotu.

Ali-runbookin kutsutaan tekstiin parametrit voi olla mikä tahansa tietotyyppi, mukaan lukien monimutkaisia objekteja ja ei ole [JSON Sarjatoiminto](automation-starting-a-runbook.md#runbook-parameters) on käynnistyessä Azure hallinta-portaalissa runbookin tai Käynnistä AzureRmAutomationRunbook cmdlet-komento.


### <a name="runbook-types"></a>Runbookin tyypit

Mitä videotyyppejä voit soittaa toisistaan:

- [PowerShellin runbookin](automation-runbook-types.md#powershell-runbooks) ja [graafiset runbooks](automation-runbook-types.md#graphical-runbooks) voi soittaa kunkin tekstiin (molemmat ovat PowerShell mukaan).
- [PowerShell-työnkulun runbookin](automation-runbook-types.md#powershell-workflow-runbooks) ja graafisia PowerShell-työnkulun runbooks soittaa kunkin tekstiin (molemmat ovat mukaan PowerShell-työnkulku)
- PowerShell ja PowerShell työnkulun tietotyypeistä et voi soittaa toisiinsa tekstiin ja Käynnistä AzureRmAutomationRunbook on käytettävä.
    
Kun julkaiseminen tilauksen asiaa:

- Julkaise järjestyksen runbooks asioissa vain PowerShell ja graafisia PowerShell-työnkulku runbooks varten.


Kun soitat graafiset tai PowerShell työnkulun lapsen runbookin käyttämällä sisäisiä suorittamisen, voit käyttää vain: n runbookin nimi.  Soittaessasi PowerShell-lapsen runbookin sen nimi, jossa on vastaava *.\\ * Jos haluat määrittää, että komentosarja sijaitsee paikallisen kansion. 

### <a name="example"></a>Esimerkki

Seuraavassa esimerkissä käynnistää testi lapsen runbookin, joka hyväksyy kolme parametrit, monimutkaisia objektin kokonaisluku tai totuusarvon. Lapsen runbookin tulosteen määritetään muuttuja.  Tässä tapauksessa lapsen runbookin on PowerShell työnkulun runbookin

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true

Seuraavassa on käyttämällä PowerShell-runbookin lapsen edellisen esimerkin.

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = .\PS-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true



##  <a name="starting-a-child-runbook-using-cmdlet"></a>Aloitetaan ali-runbookin cmdlet-komennolla

[Käynnistä AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) cmdlet-komennon avulla voit aloittaa runbookin kuvatulla tavalla [Voit käynnistää runbookin Windows PowerShellin avulla](../automation-starting-a-runbook.md#starting-a-runbook-with-windows-powershell). Kahdessa käytön cmdlet tilassa.  Yksi tilassa cmdlet-komento palauttaa työn tunnus heti, kun lapsen työn luodaan lapsen runbookin.  Muita tilassa, joka otetaan käyttöön määrittämällä **-Odota** parametrin cmdlet odottaa lapsen projektin päättymistä ja palauttavat tulosteen lapsen runbookin.

Työn ali-runbookin aloittaminen cmdlet-komento, suoritetaan erillisessä työn ylemmän tason runbookin. Tuloksena on enemmän kuin käynnistettäessä runbookin sidottua töitä, ja tekee niistä vaikea seurata. Ylemmän tason aloittaa useita alatason runbooks asynkronisesti odottamatta kunkin suorittamiseen. Kyseisen samanlaisia rinnakkain suorittamisen kutsumista lapsen runbooks sidottua ylemmän tason runbookin on [Rinnakkainen avainsana](automation-powershell-workflow.md#parallel-processing).

Parametrien ali-runbookin aloittaminen cmdlet-komento, on esitetty Hajautustaulukkoa kuvatulla tavalla [Runbookin parametrit](automation-starting-a-runbook.md#runbook-parameters). Vain yksinkertaisia tietotyyppejä voidaan. Jos n runbookin on monimutkaisia tietotyypin parametrin, valitse se on kutsuttava tekstiin.

### <a name="example"></a>Esimerkki

Seuraavassa esimerkissä lapsen runbookin alkaa parametrit ja odottaa sen valmistumista käyttämällä aloitus-AzureRmAutomationRunbook-parametrin odota. Kun suoritettu, tulos kerätään lapsen runbookin.

    $params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true} 
    $joboutput = Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-ChildRunbook" -ResouceGroupName "LabRG" –Parameters $params –wait


## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>Menetelmien kutsumista lapsen runbookin vertailu

Seuraavassa taulukossa on yhteenveto kahdella tavalla runbookin soitettaessa toiseen runbookin välisiä eroja.

| | Tekstiin| Cmdlet-komento|
|:---|:---|:---|
|Työn|Lapsen runbooks suorittaa saman työn kuin ylätason.|Erillinen työn luodaan lapsen runbookin.|
|Suorittaminen|Ylemmän tason runbookin odottaa lapsen runbookin päättymistä, ennen kuin jatkat.|Ylemmän tason runbookin säilyy heti, kun lapsen runbookin käynnistetään *tai* ylemmän tason runbookin odottaa lapsen projektin loppuun.|
|Tulos|Ylemmän tason runbookin suoraan saa tulosteen lapsen runbookin.|Ylemmän tason runbookin hakemalla tulosteen lapsen runbookin työn *tai* ylemmän tason runbookin suoraan saa tulosteen lapsen runbookin.|
|Parametrit|Lapsen runbookin parametrien arvot on määritetty erikseen, ja voit käyttää mitä tahansa tietotyyppi.|Arvojen lapsen runbookin parametrit on voidaan yhdistää yhden hajautustaulukko ja voi sisältää vain yksinkertaisia, taulukko ja objektin tietotyyppejä, suorituskykykertoimen JSON Sarjatoiminto.|
|Automaatio-tili|Ylemmän tason runbookin käyttää vain lapsen runbookin samaa automaatio-tiliä.|Ylemmän tason runbookin voit käyttää alemman tason runbookin saman Azure tilaus ja eri tilauksen minkä tahansa automaatio-tililtä, jos sinulla on yhteys siihen.|
|Julkaiseminen|Lapsen runbookin on julkaistava, ennen kuin ylemmän tason runbookin on julkaistu.|Lapsen runbookin on julkaistava milloin tahansa ennen ylemmän tason runbookin aloitetaan.|

## <a name="next-steps"></a>Seuraavat vaiheet

- [Tietojen runbookin Azure automaatio](automation-starting-a-runbook.md)
- [Runbookin tulostus ja Azure Automaattiset viestien](automation-runbook-output-and-messages.md)
