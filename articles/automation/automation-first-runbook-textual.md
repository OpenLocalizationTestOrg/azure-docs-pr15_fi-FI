<properties
    pageTitle="Oma ensimmäisen PowerShell työnkulun runbookin Azure automaatio | Microsoft Azure"
    description="Opetusohjelma, joka opastaa sinua luonti-testaaminen ja julkaiseminen sekä pelkkänä tekstinä-runbookin PowerShell-työnkulun avulla."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""
    keywords="työnkulun PowerShell powershell työnkulun esimerkkejä työnkulun powershell"/>
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="magoedte;bwren"/>

# <a name="my-first-powershell-workflow-runbook"></a>Oma ensimmäisen työnkulun PowerShell-runbookin

> [AZURE.SELECTOR] - [Graafisen](automation-first-runbook-graphical.md) - [PowerShell](automation-first-runbook-textual-PowerShell.md) - [PowerShell-työnkulku](automation-first-runbook-textual.md)

Tässä opetusohjelmassa esitellään [PowerShell työnkulun runbookin](automation-runbook-types.md#powerShell-workflow-runbooks) Azure automaatio luomista. Asetetaan ensin yksinkertainen runbookin, jossa on Testaa ja Julkaise, kun kerromme runbookin työn tilan seuraaminen. Valitse muokkaamalla hallittavan todella Azure resurssien runbookin alkaen tällöin Azure virtuaalikoneen. Olemme Tee: n runbookin tehokkaamman lisäämällä runbookin parametreja.

## <a name="prerequisites"></a>Edellytykset

Jotta voit suorittaa tässä opetusohjelmassa, sinun on seuraavasti.

-   Azure tilaus. Jos jokin ei ole vielä, voit [aktivoida MSDN-tilaaja edut](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) tai <a href="/pricing/free-account/" target="_blank"> [rekisteröityä maksuttoman tilin](https://azure.microsoft.com/free/).
-   [Automaatio-tili](automation-security-overview.md) : n runbookin ja Azure resurssien todennusta.  Tällä tilillä on oikeus aloittaa ja lopettaa virtuaalikoneen.
-   Azure virtual machine. Lopeta ja Käynnistä tämän tietokoneen, jotta se tulee tuotannon.

## <a name="step-1---create-new-runbook"></a>Vaihe 1 - Luo uusi runbookin

Asetetaan ensin luomalla yksinkertaisen runbookin, tulostaa tekstiä *Hei maailma*.

1.  Avaa Azure-portaaliin, automaatio-tilillesi.  
    Automaatio-tili-sivu tutustutaan Pikakatselu resursseista tähän tiliin. On jo joitakin resurssit. Useimmat ne ovat moduulit, jotka sisältyvät automaattisesti uuden automaatio-tilin. On myös oltava tunnistetiedon resurssi, joka on kuvattu [edellytykset](#prerequisites).
2.  Valitse Avaa runbooks luettelo **Runbooks** -ruutu.<br> ![Runbooks hallinta](media/automation-first-runbook-textual/runbooks-control.png)
3.  Luo uusi runbookin valitsemalla **Lisää runbookin** -painiketta ja **Luo uusi runbookin**.
4.  Anna: n runbookin nimi *MyFirstRunbook työnkulun*.
5.  Tässä tapauksessa emme aiot luoda [PowerShell työnkulun runbookin](automation-runbook-types.md#powerShell-workflow-runbooks) **Powershell työnkulun** valitseminen niin **Runbookin tyyppi**.<br> ![Uusi runbookin](media/automation-first-runbook-textual/new-runbook.png)
6.  Valitse **Luo** luomaan: n runbookin ja tekstiä editorin avaaminen.

## <a name="step-2---add-code-to-the-runbook"></a>Vaihe 2 – koodin lisääminen: n runbookin

Voit joko koodi suoraan: n runbookin, tai voit valita cmdlet-komennot, runbooks ja varat kirjaston ohjausobjektista ja että ne lisätään runbookin kaikki liittyvät parametreilla. Näiden vaiheiden olemme kirjoittaa suoraan: n runbookin.

1.  Tutustu runbookin on tällä hetkellä tyhjä tarvittavat *työnkulun* -avainsana, tutustu runbookin ja aaltosulkeet, joka encase koko työnkulun nimi. 

    ```
    Workflow MyFirstRunbook-Workflow
    {
    }
    ```

2.  Kirjoita *Kirjoita tulosteen "Tervetuloa maailman."* sulkeissa. 
   
    ```
    Workflow MyFirstRunbook-Workflow
    {
      Write-Output "Hello World"
    }
    ```

3.  Tallenna: n runbookin valitsemalla **Tallenna**.<br> ![Tallenna runbookin](media/automation-first-runbook-textual/runbook-edit-toolbar-save.png)

## <a name="step-3---test-the-runbook"></a>Vaihe 3: n runbookin testi

Ennen kuin on julkaista olla käytettävissä tuotannon runbookin, haluat testata ja varmistaa, että se toimii oikein. Kun testaat runbookin, suorita sen **Luonnokset** -versio ja tarkastella sen Tulosta vuorovaikutteisesti.

1.  Valitse **Testaa ruudun** Avaa testi-ruutu.<br> ![Testi-ruutu](media/automation-first-runbook-textual/runbook-edit-toolbar-test-pane.png)
2.  Valitse Aloita testi **Käynnistä-painiketta** . Tämä on oltava käytössä ainoa vaihtoehto.
3.  [Runbookin työn](automation-runbook-execution.md) luodaan ja sen tilan näkyviin.  
    Työn tila alkaa kuin *jonossa* , joka ilmaisee, että se odottaa pilveen runbookin työntekijä tulee käytettävissä. Se sitten siirtyy *käynnistetään* kun työntekijä väittää työn ja *suorittamalla* , kun n runbookin todella käynnistyy.  
4.  Kun runbookin työ on valmis, tulos tulee näkyviin. Tässä tapauksessa emme pitäisi näkyä *Hei maailma*.<br> ![Moi maailma](media/automation-first-runbook-textual/test-output-hello-world.png)
5.  Sulje palaa alusta testi-ruutu.

## <a name="step-4---publish-and-start-the-runbook"></a>Vaihe 4 – Julkaise ja Käynnistä: n runbookin

Runbookin, jonka juuri luonut on edelleen luonnoksena. Julkaise se ennen kuin olemme voidaan suorittaa tuotannon annettava. Kun julkaiset runbookin, korvaa aiemmin julkaistu versio luonnos-versiolla. Tässä tapauksessa emme ole julkaistu versio vielä koska juuri luomaasi: n runbookin.

1.  Valitse **Julkaise** , jos haluat julkaista: n runbookin ja sitten **Kyllä** pyydettäessä.<br> ![Julkaiseminen](media/automation-first-runbook-textual/runbook-edit-toolbar-publish.png)
2.  Voit nyt tarkastella: n runbookin **Runbooks** -ruudussa Vierittää vasemmalle, jos se näkyy **Yhtä aikaa muiden kanssa tila** on **julkaistu**.
3.  Vieritä takaisin oikeuden tarkastella ruudun **MyFirstRunbook**työnkulun.  
    Yläreunassa vaihtoehtojen avulla, että aloittaa: n runbookin, Ajoita sen myöhemmin jonkin aikaa alkamaan tai luo [webhook](automation-webhooks.md) , jotta se voidaan aloittaa HTTP-puhelun kautta.
4.  Haluamme vain käynnistää: n runbookin niin Valitse **Käynnistä** ja valitse sitten **Kyllä** pyydettäessä.<br> ![Käynnistä runbookin](media/automation-first-runbook-textual/runbook-toolbar-start.png)
5.  Työruutu avataan runbookin, jonka juuri luonut projektille. Olemme Sulje tämä ruutu, mutta tässä tapauksessa emme jätä se avatuksi niin emme voi katsella projektin edistymisen.
6.  Työn tilana näkyy **Yhteenveto** ja käyttää vastineena tilat, joka on tuli kun testattu: n runbookin.<br> ![Projektin yhteenveto](media/automation-first-runbook-textual/job-pane-summary.png)
7.  Kun runbookin tila on *Valmis*, valitse **tulos**. Tulostus-ruutu on auki ja näkyvissä Microsoftin *Hei maailma*.<br> ![Projektin yhteenveto](media/automation-first-runbook-textual/job-pane-output.png)  
8.  Sulje tulostus-ruutu.
9.  Valitse **virtaa** Avaa runbookin työn virtaa-ruutu. Näemme olisi vain *Hei maailma* tulostus-muodossa, mutta tämä Näytä muut virtaa runbookin työlle, kuten yksityiskohtainen ja virheen, jos n runbookin kirjoittaa ne.<br> ![Projektin yhteenveto](media/automation-first-runbook-textual/job-pane-streams.png)
10. Sulje virtaa-ruutu ja palaa MyFirstRunbook-ruudussa työ-ruutu.
11. Valitse **työt** Avaa tämä runbookin työt-ruutu. Tämä näyttää luettelon kaikista tämän runbookin luoma työt. Näemme on vain yksi työ, koska on vain suoritettiin työn kerran luettelossa.<br> ![Projektit](media/automation-first-runbook-textual/runbook-control-jobs.png)
12. Voit valita tämän työn Avaa sama työ-ruutu, jossa on tarkastellut olemme käynnistyessään: n runbookin. Näin voit siirtyä takaisin aika ja tarkastella projektin, joka on luotu tietyn runbookin.

## <a name="step-5---add-authentication-to-manage-azure-resources"></a>Vaihe 5 – Lisää resurssien Azure-todennus

Syy on testannut ja julkaista Microsoftin runbookin, mutta tähän mennessä se ei tee mitään hyötyä. Haluat sen Azure resurssien. Se voi tehdä mutta ellei on se todennetaan, joihin viitataan [edellytykset](#prerequisites)-tunnistetiedoilla. Olemme tehdä, **Lisää AzureRMAccount** cmdlet-komento.

1.  Avaa tekstiä editori valitsemalla **Muokkaa** MyFirstRunbook työnkulun-ruudussa.<br> ![Muokkaa runbookin](media/automation-first-runbook-textual/runbook-toolbar-edit.png)
2.  Microsoft ei enää tarvita **Kirjoitus-tulostus** -rivin, joten siirry eteenpäin ja poista se.
3.  Vie kohdistin kohtaan sulkeissa tyhjälle riville.
4.  Kirjoita tai kopioi ja liitä seuraava koodi, joka käsittelee todennus tilisi automaatio Suorita nimellä:

    ```
    $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    ```

5.  Valitse **Testaa ruutu** , jotta voimme testata: n runbookin.
6.  Valitse Aloita testi **Käynnistä-painiketta** . Kun se on valmis, näyttöön tulee tulosteen samalla seuraavat, jossa on näkyvissä perustiedot-tililtä. Tämä tarkoittaa, että tunnistetieto on kelvollinen.<br> ![Todennus](media/automation-first-runbook-textual/runbook-auth-output.png)

## <a name="step-6---add-code-to-start-a-virtual-machine"></a>Vaihe 6 – Aloita virtual machine-koodin lisääminen

Nyt kun meidän runbookin todentaa Microsoftin Azure tilaukseen, on hallita resursseja. Lisäämme komennon käynnistettäessä. Voit valita minkä tahansa virtual machine Azure-tilaukseesi, ja nyt olemme on hardcoding, johon funktio kyselyjä-cmdlet-komennolla.

1.  Perään *Lisää AzureRmAccount* *Käynnistä AzureRmVM-nimen "VMName" - ResourceGroupName 'NameofResourceGroup'* nimelle ja aloita virtuaalikoneen resurssiryhmä nimi.  

    ```
    workflow MyFirstRunbook-Workflow
    {
      $Conn = Get-AutomationConnection -Name AzureRunAsConnection
      Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
      Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName'
    }
    ``` 

2.  Tallenna: n runbookin ja valitse sitten **ruudun Testaa** , niin, että olemme voit kokeilla sitä.
3.  Valitse Aloita testi **Käynnistä-painiketta** . Kun se on valmis, tarkista, että virtuaalikoneen aloitettiin.

## <a name="step-7---add-an-input-parameter-to-the-runbook"></a>Vaihe 7 – syöteparametria lisääminen: n runbookin

Tutustu runbookin tällä hetkellä käynnistyy virtuaalisen koneen, että olemme: n runbookin englanninkielisissä, mutta se olla hyödyllisempi Jos olemme määrittää virtuaalikoneen, jos n runbookin aloitetaan. On nyt lisätään kyseiseen toimintoja runbookin syöteparametrit.

1.  Lisää parametrit *VMName* ja *ResourceGroupName* : n runbookin ja muuttujia käyttäminen **Käynnistä AzureRmVM** cmdlet-komento, kuten alla olevassa esimerkissä. 

    ```
    workflow MyFirstRunbook-Workflow
    {
       Param(
        [string]$VMName,
        [string]$ResourceGroupName
       )  
     $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
     Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
     Start-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
    }
    ```

2.  Tallenna: n runbookin ja avaa testi-ruutu. Huomaa, että arvot voidaan lisätä nyt kaksi syötteen muuttujat, joita käytetään testi.
3.  Sulje Testaa-ruutu.
4.  Valitse **Julkaise** : n runbookin uuden version julkaiseminen.
5.  Lopeta virtuaalikoneen, jotka olet aloittanut edellisessä vaiheessa.
6.  Valitse Aloita: n runbookin **Käynnistä-painiketta** . Kirjoita **VMName** ja **ResourceGroupName** virtuaalikoneen, jotka aiot Käynnistä-painiketta.<br> ![Käynnistä Runbookin](media/automation-first-runbook-textual/automation-pass-params.png)

7.  Kun n runbookin on valmis, tarkista, että virtuaalikoneen aloitettiin.

## <a name="next-steps"></a>Seuraavat vaiheet

-  Graafinen runbooks aloittaminen-kohdassa [Oma ensimmäisen graafinen runbookin](automation-first-runbook-graphical.md)
-  Aloita PowerShell runbooks-kohdassa [Omat ensimmäisen PowerShell-runbookin](automation-first-runbook-textual-powershell.md)
-  Lisätietoja runbookin sekä niiden eduista ja rajoituksista on artikkelissa [Azure automaatio runbookin tiedostotyypit](automation-runbook-types.md)
-  Lisätietoja PowerShell-komentosarjaa ominaisuus tuesta on kohdassa [alkuperäisen PowerShell-komentosarjaa Azure automaatio tuki](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)