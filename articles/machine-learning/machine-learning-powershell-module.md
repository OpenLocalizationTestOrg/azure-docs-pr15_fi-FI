<properties
    pageTitle="Koneen Learning PowerShell-moduuli | Microsoft Azure"
    description="Azure koneen Learning PowerShell-moduuli on käytettävissä julkisen esikatselu-tilassa. PowerShellin avulla voit luoda ja hallita työtilat, tutkimuksiin tai verkkopalveluihin."
    keywords="Kokeile lineaarisen regressiosuoran, tietokoneen learning algoritmeista, tietokoneen learning opetusohjelma ennakoivan mallinnus tekniikoita tietojen tiede kokeen"
    services="machine-learning"
    documentationCenter=""
    authors="hning86"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/05/2016"
    ms.author="garye;haining"/>

# <a name="powershell-module-for-microsoft-azure-machine-learning"></a>Microsoft Azure koneen Learning PowerShell-moduuli

Azure koneen Learning PowerShell-moduuli on tehokas työkalu, jonka avulla voit hallita työtilat, kokeet, tietojoukkoja, web serivces ja lisää Windows PowerShellin avulla.

Voit tarkastella asiakirjoista ja lataa moduuliin sekä koko lähdekoodi osoitteessa [https://aka.ms/amlps](https://aka.ms/amlps). 

## <a name="what-is-the-machine-learning-powershell-module"></a>Mikä on tietokoneen Learning PowerShell-moduulin?

Koneen Learning PowerShell-moduulin on. VERKON perustuva DLL-moduulia, jonka avulla voit hallita täysin Windows PowerShellin Azure koneen Learning työtilat, kokeet, tietojoukkoja, verkkopalveluita ja WWW-Palvelupäätepisteet. Sekä moduulin voit ladata koko lähdekoodin puhtaasti määritetty vastaamaan erotettu [C# API kerroksen](https://github.com/hning86/azuremlps/blob/master/code/AzureMLSDK.cs)mukaan lukien. Tämä tarkoittaa sitä, voit viitata dll-tiedoston, .NET-projektin ja hallita Azure koneen Learning .NET-koodin avulla. Lisäksi DLL määräytyy pohjana REST API, joka voidaan hyödyntää suoraan tuttuja asiakkaalle.

## <a name="what-can-i-do-with-the-powershell-module"></a>Mitä voin tehdä PowerShell-moduulin kanssa?

Seuraavassa on joitakin tehtäviä, voit suorittaa tämän PowerShell-moduulin kanssa. Tutustu [täydelliset asiakirjat](https://aka.ms/amlps) nämä ja monia muita toimintoja.

- Uuden työtilan luominen käyttämällä hallinta-varmenteen ([Uusi AmlWorkspace](https://github.com/hning86/azuremlps#new-amlworkspace)) valmistelu
- Vieminen ja tuominen JSON tiedoston edustava koe-kaavio ([Vie AmlExperimentGraph](https://github.com/hning86/azuremlps#export-amlexperimentgraph) ja [Tuo AmlExperimentGraph](https://github.com/hning86/azuremlps#import-amlexperimentgraph))
- Kokeilla ([Käynnistä AmlExperiment](https://github.com/hning86/azuremlps#start-amlexperiment))
- Luo verkkopalvelun ulos ennakoivan kokeen ([Uusi AmlWebService](https://github.com/hning86/azuremlps#new-amlwebservice))
- Luo uusi päätepiste julkaistun web-palveluun ([Lisää AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint))
- Käynnistää RRS ja/tai BES web Palvelupäätepisteen ([Käynnistä AmlWebServiceRRSEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) ja [Käynnistä AmlWebServicBESEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint))

Seuraavassa on lyhyt Esimerkki aiemmin kokeilla PowerShellin avulla:

        #Find the first Experiment named “xyz”
        $exp = (Get-AmlExperiment | where Description -eq ‘xyz’)[0]
        #Run the Experiment
        Start-AmlExperiment -ExperimentId $exp.ExperimentId 

Tarkemmin käyttötapaus on tämän artikkelin PowerShell-moduulin käyttämisestä hyvin yleisesti-pyydetty automatisointi: [luoda monta koneen Learning malleja ja web Palvelupäätepisteet-yksi kokeen PowerShellin avulla](machine-learning-create-models-and-endpoints-with-powershell.md).

## <a name="how-do-i-get-started"></a>Miten aloitetaan?

Aloita koneen Learning PowerShell- [Vapauta paketin](https://github.com/hning86/azuremlps/releases) lataaminen GitHub ja noudata [ohjeita asennuksen](https://github.com/hning86/azuremlps/blob/master/README.md). Sinun on eston ladata/purettuna dll-Kirjaston ja tuo ne PowerShell-ympäristöön. Useimmat Cmdlet-komentoja edellyttää, että annat työtila-tunnus, työtilan luvan tunnuksen ja Azure alue, joka on työtilan. Helpoin tapa antaa nämä on oletusarvoinen config.json tiedosto, joka peittää yksityiskohtaisesti asennusohjeita. Voit myös Kloonaa git puun ja muokata tai kääntää koodin Visual Studiossa paikallisesti.

## <a name="next-steps"></a>Seuraavat vaiheet

PowerShell-moduulin säilyvät parannettu ja laajennettu esikatselu tänä aikana. Seuraa [Cortana liiketoimintatietojen ja tietokoneen Learning blogi](https://blogs.technet.microsoft.com/machinelearning/) on uutisia ja lisätietoja.
