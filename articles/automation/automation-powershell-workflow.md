<properties 
   pageTitle="Oppimiskeskuksen PowerShell-työnkulku"
   description="Tässä artikkelissa on tarkoitettu kuin nopeasti Harjoitus tekijät tuttuja PowerShell ymmärtää PowerShell ja PowerShell työnkulun tietyn erot."
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
   ms.date="09/12/2016"
   ms.author="bwren" />

# <a name="learning-windows-powershell-workflow"></a>Learning Windows PowerShell-työnkulku

Azure automaatio Runbooks on toteutettu Windows PowerShellin työnkulkuja.  Windows PowerShell-työnkulun muistuttaa Windows PowerShell-komentosarjaa, mutta se on joitakin merkittäviä eroja, joka voi olla hankalaa uudelle käyttäjälle.  Tässä artikkelissa on tarkoitettu käyttäjille, jotka ovat jo aiemmin PowerShell ja kuvataan lyhyesti käsitteitä edellyttävät Jos muunnat PowerShell-komentosarjaa PowerShell-työnkulun runbookin käytettäviksi.  

Työnkulun on ohjelmoitua, yhdistetyt vaiheet, jotka suorittavat pitkään suoritettavien tehtävien tai useita vaiheita yhteensovittamisesta edellyttää useiden laitteiden tai hallitun solmujen järjestyksessä. Työnkulun normaali komentosarjan kautta etuja ovat voi suorittaa samanaikaisesti useilla eri laitteilla vastaan ja mahdollisuuden pitää palauttaminen virheet automaattisesti. Windows PowerShell-työnkulku on Windows PowerShell-komentosarja, joka käyttää Windows Workflow Foundation. Kun työnkulku on kirjoitettu Windows PowerShell-syntaksin ja käynnistää Windows PowerShell, se käsitellään Windows Workflow Foundation.

Valmis lisätietoja tämän artikkelin aiheista on artikkelissa [Windows PowerShellin työnkulun käytön aloittaminen](http://technet.microsoft.com/library/jj134242.aspx).

## <a name="types-of-runbook"></a>Runbookin tyypit

On kolmenlaisia runbookin Azure automaatio, *PowerShell-työnkulun*, *PowerShell* ja *graafisia*.  Voit määrittää runbookin tyyppi, kun luot: n runbookin ja runbookin ei voi muuntaa toisen tyypin, kun se on luotu.

PowerShellin työnkulun runbooks ja PowerShell runbooks käyttäjille, jotka haluat työskennellä suoraan PowerShell-koodia sisältävien joko käyttävät tekstimuotoinen editorin Azure automaatio- tai offline-tilassa editorin, kuten PowerShell ise:. Tämän artikkelin tiedot pitäisi ymmärtää, jos olet luomassa PowerShell työnkulun runbookin. 

Graafisen runbooks avulla voit luoda runbookin käytät samoja toimintoja ja Cmdlet-komentoja ei graafisessa käyttöliittymässä, joka piilottaa pohjana PowerShell-työnkulun monimutkaisia.  Käsitteistä, kuten tarkistuspisteet ja rinnakkaisia suorittamisen tämän artikkelin silti käyttää graafinen runbooks, mutta sinun ei tarvitse olla huolissaan siitä, syntaksista. 

## <a name="basic-structure-of-a-workflow"></a>Työnkulun perusrakenteen

Ensimmäinen vaihe PowerShell-komentosarjaa muuntaminen PowerShell työnkulun kirjoittamalla se **työnkulun** -avainsanalla.  Työnkulku alkaa **työnkulun** avainsanan aaltosulkeiden komentosarja tekstiosaan. Työnkulun nimen jäljessä **työnkulun** avainsana seuraavan syntaksin mukaisesti. 

    Workflow Test-Workflow
    {
       <Commands>
    }

Työnkulun nimen on oltava samat automaatio-runbookin nimi. Jos n runbookin tuodaan, tiedostonimi on vastattava työnkulun nimi ja lopussa on oltava .ps1.

Jos haluat lisätä parametreja työnkulun, käytä **parametri** -avainsanaa samalla tavalla kuin komentosarjan. 

## <a name="code-changes"></a>Muutokset

PowerShellin työnkulun koodi näyttää lähes samoin kuin PowerShell-komentosarjoja lukuun ottamatta muutamia merkittäviä muutoksia.  Seuraavissa kohdissa kuvataan muutokset, jotka haluat tehdä PowerShell-komentosarja sen työnkulun suorittaminen.

### <a name="activities"></a>Toiminnot

Tehtävä on tietyn tehtävän työnkulun. Samalla tavalla kuin komentosarjan koostuu vähintään yksi komentoja, työnkulun koostuu vähintään yksi toimintoja, jotka suoritetaan järjestyksessä. Windows PowerShellin työnkulun muuntaa automaattisesti monia Windows PowerShellin cmdlet-komennot toimintoja suorittaessaan työnkulun. Kun määrität oman runbookin jokin näistä cmdlet-komennot, vastaavan aktiviteetin todella Suorita Windows Workflow Foundation mukaan. Windows PowerShell-työnkulku suoritetaan automaattisesti ilman vastaavaa aktiviteettia näiden cmdlet-cmdlet-komento, [InlineScript](#inlinescript) aktiviteetin. On joukko cmdlet-komennot, jotka eivät sisälly eikä sitä voi käyttää työnkulun, ellet nimenomaisesti, että Sisällytä ne InlineScript estoasetukset. Saat lisätietoja käsitteistä, [Komentosarjaa työnkulut käyttämällä toimintoja](http://technet.microsoft.com/library/jj574194.aspx).

Työnkulun toiminnot jakaminen yleisten parametrien määrittäminen toiminnan määrittäminen. Lisätietoja työnkulun yhteiset parametrit-kohdassa [about_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx).

### <a name="positional-parameters"></a>Sijainti parametrit

Et voi käyttää sijainti parametrit toiminnot ja työnkulun cmdlet-komennoista.  Tämä tarkoittaa on, sinun on käytettävä parametrinimet.

Kuvitellaan seuraava koodi, joka hakee kaikki käynnissä olevat palvelut.

     Get-Service | Where-Object {$_.Status -eq "Running"}

Jos yrität suorittaa tämän saman koodin työnkulussa, saat viestin, kuten "parametrin määrittäminen ei voi selvittää käyttämällä määritettyä nimettyjä parametreja."  Voit korjata ongelman poistamalla on kuin seuraavassa parametrin nimi.

    Workflow Get-RunningServices
    {
        Get-Service | Where-Object -FilterScript {$_.Status -eq "Running"}
    }

### <a name="deserialized-objects"></a>Sarjoitus objektit

Objektien työnkuluissa ovat poistaa.  Tämä tarkoittaa, että niiden ominaisuudet ovat käytettävissä, mutta ei niiden tavoista.  Kuvitellaan seuraava PowerShell-koodi, joka lopettaa palvelun palvelun objektin Stop-menetelmää.

    $Service = Get-Service -Name MyService
    $Service.Stop()

Jos yrität suorittaa työnkulussa, saat virheen, jossa sanotaan "menetelmän kutsu ei ole tuettu Windows PowerShell-työnkulun".  

Yksi vaihtoehto on nämä kaksi riviä koodin rivittää [InlineScript](#InlineScript) -lohko, jolloin $Service olisi service-objektin sisällä lohkon. 

    Workflow Stop-Service
    {
        InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
        }
    } 

Toinen vaihtoehto on käyttää toisen cmdlet-komento, joka suorittaa samalla tavalla kuin menetelmää, jos sellainen on käytettävissä.  Tässä esimerkissä Pysäytä palvelu cmdlet-komento toimii samalla tavalla kuin Pysäytä menetelmä, ja voit käyttää seuraavia työnkulun.

    Workflow Stop-MyService
    {
        $Service = Get-Service -Name MyService
        Stop-Service -Name $Service.Name
    }


## <a name="inlinescript"></a>InlineScript

**InlineScript** tehtävän on hyödyllinen, kun haluat suorittaa yhden tai useamman komennot perinteinen PowerShell-komentosarjaa PowerShell työnkulun sijaan.  Windows Workflow Foundation lähetetään työnkulun komentojen käsittelyä varten, kun komennot InlineScript estoasetukset käsitellään Windows PowerShellin avulla. 

InlineScript käyttää alla syntaksia.

    InlineScript
    {
      <Script Block>
    } <Common Parameters>

Voit palauttaa tulosteen InlineScript määrittämällä tulosteen muuttujan. Seuraavassa esimerkissä lopettaa palvelun ja tulostaa sitten palvelunimi.

    Workflow Stop-MyService
    {
        $Output = InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


Voit siirtää arvot InlineScript lohko, mutta sinun on käytettävä **$Using** laajuus-määrite.  Seuraavassa esimerkissä on sama kuin edellisessä esimerkissä, sillä erotuksella, että palvelunimi on annettu muuttujan. 

    Workflow Stop-MyService
    {
        $ServiceName = "MyService"
    
        $Output = InlineScript {
            $Service = Get-Service -Name $Using:ServiceName
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


Kun InlineScript toiminnot voivat olla kriittinen tiettyjä työnkulkuja, eivät tue työnkulun rakenteita ja kannattaa käyttää vain tarvittaessa seuraavista syistä:

- Et voi käyttää [tarkistuspisteet](#Checkpoints) sisältyy InlineScript estä. Virheen ilmetessä alueen sisällä se täytyy jatkaa aikalohkon alusta.
- Et voi käyttää InlineScriptBlock sisältyy [rinnakkain suorittamisen](#parallel-execution) .
- InlineScript vaikuttaa skaalattavuus työnkulun, koska se sisältää Windows PowerShell-istunnon koko pituus InlineScript aikalohkon.

Lisätietoja InlineScript käyttämisestä on [Käynnissä Windows PowerShell-komennoilla työnkulussa](http://technet.microsoft.com/library/jj574197.aspx) ja [about_InlineScript](http://technet.microsoft.com/library/jj649082.aspx).


## <a name="parallel-processing"></a>Rinnakkainen käsittely

Windows PowerShell-työnkulkujen yhden etuna on se voi suorittaa komentojen rinnakkain sijaan peräkkäin samalla tavalla kuin tavallinen komentosarjan. 

**Rinnakkaisia** avainsana voit luoda komentosarjan eston useita komentoja, jotka suoritetaan samanaikaisesti. Tämä käyttää alla syntaksia. Tässä tapauksessa Activity1 ja Activity2 alkavat samaan aikaan. Activity3 alkaa vasta, kun Activity1 ja Activity2 on valmis.

    Parallel
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>


Esimerkkinä seuraavat PowerShell-komennot, jotka kopioida useita tiedostoja verkossa kohteeseen.  Nämä komennot suoritetaan järjestyksessä, niin kuin yhden tiedoston on valmis ennen aloittamista seuraavan kopioiminen.     

    $Copy-Item -Path C:\LocalPath\File1.txt -Destination \\NetworkPath\File1.txt
    $Copy-Item -Path C:\LocalPath\File2.txt -Destination \\NetworkPath\File2.txt
    $Copy-Item -Path C:\LocalPath\File3.txt -Destination \\NetworkPath\File3.txt

Seuraavat työnkulkua käytetään saman komentoja rinnakkain niin, että ne kaikki alkavat kopioiminen samaan aikaan.  Vain, kun ne ovat kaikki kopiointi valmistuminen viesti näkyy.

    Workflow Copy-Files
    {
        Parallel 
        {
            $Copy-Item -Path "C:\LocalPath\File1.txt" -Destination "\\NetworkPath"
            $Copy-Item -Path "C:\LocalPath\File2.txt" -Destination "\\NetworkPath"
            $Copy-Item -Path "C:\LocalPath\File3.txt" -Destination "\\NetworkPath"
        }

        Write-Output "Files copied."
    }


Voit käyttää **ForEach-rinnakkainen** rakennetta, prosessi-komentoja, kunkin kohteen kokoelman samanaikaisesti. Kohteiden kerääminen käsitellään rinnakkain samalla, kun komentosarja estoasetukset komennot suoritetaan järjestyksessä. Tämä käyttää alla syntaksia. Tässä tapauksessa Activity1 alkaa samanaikaisesti kaikkien kohteiden kerääminen. Kunkin kohteen Activity2 alkaa Activity1 päätyttyä. Activity3 alkaa vasta, kun Activity1 ja Activity2 suorittanut kaikkia muita kohteita.

    ForEach -Parallel ($<item> in $<collection>)
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>

Seuraavassa esimerkissä on samankaltainen kuin edellisessä esimerkissä kopioimista rinnakkain.  Tässä tapauksessa viestin näkyy kaikille tuotaville tiedostoille, kun se kopioi.  Vain, kun ne ovat kaikki kopiointi lopulliseen valmistumiseen viesti näkyy.

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach -Parallel ($File in $Files) 
        {
            $Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
        }
        
        Write-Output "All files copied."
    }

> [AZURE.NOTE]  Ei ole suositeltavaa käynnissä lapsen runbooks rinnakkain, koska tämä on osoitettu antaa epäluotettavista.  Lapsen runbookin joskus tulosteen eivät näy ja yksi lapsi runbookin asetukset voivat vaikuttaa rinnakkain ali-runbooks 


## <a name="checkpoints"></a>Tarkistuspisteet

*Tarkistuspiste* on nykyisen tilan työnkulun, joka sisältää nykyisen arvon muuttujat ja kaikki kyseiseen pisteeseen tulostus. Jos työnkulku päättyy virhe tai keskeytetään, sitten seuraavan kerran, se suoritetaan se aloittaa siitä sen sijaan worfklow alkuun viimeisen tarkistuspiste.  Voit määrittää tarkistuspisteen työnkulun **Tarkistuspiste työnkulku** -toimintoa.

Seuraava näyte koodissa poikkeuksen tapahtuu Activity2 jälkeen ilmenneet työnkulun loppuun. Työnkulku suoritetaan uudelleen, kun se käynnistyy suorittamalla Activity2, koska tämä on vain, kun viimeinen tarkistuspiste määritetty.

    <Activity1>
    Checkpoint-Workflow
    <Activity2>
    <Exception>
    <Activity3>

Sinun kannattaa asettaa tarkistuspisteet työnkulussa, kun toimintoja, jotka ehkä voi enää poikkeuksen ja tulee toistetaan työnkulun jatketaan. Esimerkiksi työnkulun voi luoda virtual machine. Voit asettaa tarkistuspisteen ennen ja jälkeen Luo virtuaalikoneen komentoja. Jos luominen epäonnistuu, valitse komennot toistaa Jos työnkulku aloitetaan uudelleen. Jos worfklow epäonnistuu kun luominen onnistuu, sitten virtuaalikoneen ei voi luoda uudelleen kun työnkulku on sen käyttöä jatketaan.

Seuraavassa esimerkissä useita tiedostoja kopioituu verkkosijainti ja määrittää tarkistuspisteen jälkeen tiedostoille.  Jos verkkosijainti katoaa, työnkulun päättyy virhe.  Kun se käynnistetään uudelleen, se jatkaa osoitteessa viimeisen tarkistuspiste, mikä tarkoittaa, että vain tiedostot, jotka on jo kopioitu ohitetaan.

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach ($File in $Files) 
        {
            $Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
            Checkpoint-Workflow
        }
        
        Write-Output "All files copied."
    }

Koska käyttäjänimi tunnistetietoja ei säily, kun soitat [Keskeytys-työnkulun](https://technet.microsoft.com/library/jj733586.aspx) tehtävän tai viimeisen tarkistuspiste jälkeen on määritettävä null ja sitten Nouda ne uudelleen resurssi-kaupasta jälkeen **Keskeytys työnkulun** tai tarkistuspiste kutsutaan tunnistetiedot.  Muussa tapauksessa voit saada seuraavan virhesanoman: *työnkulun voi jatkaa, joko koska pysyvyyttä tietojen saattaa ei tallennettu kokonaan, tai tallennettu pysyvyyttä tiedot on vioittunut. Työnkulku on käynnistettävä.*

Saman seuraava koodi esitellään, miten käsittelemään tämän työnkulun PowerShell-runbooks.

       
    workflow CreateTestVms
    {
       $Cred = Get-AzureAutomationCredential -Name "MyCredential"
       $null = Add-AzureRmAccount -Credential $Cred

       $VmsToCreate = Get-AzureAutomationVariable -Name "VmsToCreate"

       foreach ($VmName in $VmsToCreate)
         {
          # Do work first to create the VM (code not shown)
        
          # Now add the VM
          New-AzureRmVm -VM $Vm -Location "WestUs" -ResourceGroupName "ResourceGroup01"

          # Checkpoint so that VM creation is not repeated if workflow suspends
          $Cred = $null
          Checkpoint-Workflow
          $Cred = Get-AzureAutomationCredential -Name "MyCredential"
          $null = Add-AzureRmAccount -Credential $Cred
         }
     } 


Tämä ei ole pakollinen, jos määritetty pääasiallista palvelussa Suorita nimellä-tilillä on todennustapa.  

Saat lisätietoja tarkistuspisteet [Komentosarja-työnkulun lisääminen tarkistuspisteet](http://technet.microsoft.com/library/jj574114.aspx).


## <a name="next-steps"></a>Seuraavat vaiheet

- Aloita PowerShell työnkulun runbooks, katso [ensimmäinen PowerShell työnkulun-runbookin](automation-first-runbook-textual.md) 
