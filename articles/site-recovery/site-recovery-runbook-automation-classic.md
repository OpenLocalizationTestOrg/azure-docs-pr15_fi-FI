<properties
   pageTitle="Azure automaatio runbooks lisääminen palautus suunnitelmien | Microsoft Azure"
   description="Tässä artikkelissa kuvataan, miten Azure palauttaminen mahdollistaa nyt laajentamiseksi palautus-palvelupaketin ohjatulla Azure automaatio monimutkaisia tehtävien suorittamiseen palauttamisen Azure aikana"
   services="site-recovery"
   documentationCenter=""
   authors="ruturaj"
   manager="gauravd"
   editor=""/>

<tags
   ms.service="site-recovery"
   ms.devlang="powershell"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.workload="required"
   ms.date="10/23/2016"
   ms.author="ruturajd@microsoft.com"/>


# <a name="add-azure-automation-runbooks-to-recovery-plans---classic"></a>Lisää Azure automaatio runbooks palautus suunnitelmat – perinteinen


Tässä opetusohjelmassa kuvataan, miten Azure palauttaminen integroituu Azure automaatio antamaan laajennettavuus palautus-palvelupaketin. Palautus suunnitelmien voit orchestrate oman näennäiskoneiden suojattu käyttäen Azure palauttaminen toissijainen pilven replikointi ja Azure skenaariot replikoinnin palauttaminen. Ne helpottavat myös tekemistä esimerkiksi palautus **johdonmukaisesti tarkkoja** **toistettavien**ja **Automaattinen**. Jos epäonnistui tuntemattomasta Azure, että näennäiskoneiden päälle, Azure automaatio integrointi laajentaa palautus-suunnitelmien ja voit voidaan suorittaa runbooks, jolloin tehokkaita automaatio tehtävät.

Jos olet ei kuullut Azure automaatio vielä, Rekisteröidy [tähän](https://azure.microsoft.com/services/automation/) ja lataa niiden komentosarjamallit [tähän](https://azure.microsoft.com/documentation/scripts/). Lue lisätietoja [Azure palauttaminen](https://azure.microsoft.com/services/site-recovery/) ja niiden orchestrate palauttaminen käyttämällä palautus suunnitelmien Azure [tähän](https://azure.microsoft.com/blog/?p=166264).

Tässä lyhyessä opetusohjelmassa on näyttää, miten voit integroida Azure automaatio runbooks palautus suunnitelmien. Yksinkertainen, joka vaaditaan aiemmin manuaalinen toimia tehtävien automatisointi ja katso, miten voit muuntaa usean vaiheen palautuksen yhdellä napsautuksella palautus-toiminto. On myös näyttää, miten voit vianmääritys yksinkertaisen, jos se vikaan.

## <a name="protect-the-application-to-azure"></a>Azure sovelluksen suojaaminen

Anna meidän alkavat yksinkertaisen sovelluksen, joka koostuu kahdesta näennäiskoneiden. Tässä on Fabrikam HRweb soveltaminen. Fabrikam-HRweb-edusta- ja Fabrikam-Hrweb-taustatietokannan on kaksi näennäiskoneiden suojattu Azure käyttämällä Azure palauttaminen. Suojaaminen käyttämällä Azure palauttaminen näennäiskoneiden, noudata seuraavia ohjeita.

1.  Ota yhteyttä näennäiskoneiden suojaus käyttöön.

2.  Varmista, näennäiskoneiden suorittanut ensimmäisen replikoinnin ja ovat replikoiminen.

3.  Odota, kunnes ensimmäinen replikointi on valmis, ja replikoinnin tilan teksti suojattu.

![](media/site-recovery-runbook-automation/01.png)
---------------------

Tässä opetusohjelmassa luodaan automaattisesti Azure sovelluksen Fabrikam HRweb sovelluksen palautus suunnitteleminen. Valitse olemme integroida runbookin, joka luo päätepisteen-epäonnistunutta Azure virtual machine yhteyshenkilönä porttia 80, web-sivujen kautta.

Ensin luodaan tämän sovelluksen palautus suunnitteleminen.

## <a name="create-the-recovery-plan"></a>Luo palautus-suunnitelma

Palauttaa Azure sovelluksen, tarvitset palautus Suunnittele.
Voit käyttää palautus-palvelupaketti, voit määrittää näennäiskoneiden palautus järjestyksessä. Ryhmän 1 sijoitetaan virtuaalikoneen palauttaa ja aloita ensimmäisen ja virtuaalikoneen 2-ryhmässä noudata.

Luo palautus suunnitteleminen, joka näyttää alla.

![](media/site-recovery-runbook-automation/12.png)

Lisätietoja tietoja palautus-palvelupaketti, lue ohjeet [tähän](https://msdn.microsoft.com/library/azure/dn788799.aspx "tähän").

Seuraavaksi luodaan tarvittavat palvelutiedot Azure automaatio.

## <a name="create-the-automation-account-and-its-assets"></a>Automaatio-tili ja sen varat luominen

Tarvitset Azure automaatio-tilin luominen runbooks. Jos sinulla ei ole tiliä, siirry Azure automaatio-välilehti, jotka on merkitty ![](media/site-recovery-runbook-automation/02.png)ja luo uusi tili.

1.  Anna tilille nimi ja laji.

2.  Määritä maantieteellinen alue, johon haluat sijoittaa tili.

On suositeltavaa tilin sijoittaminen sama alue kuin ASR säilö.

![](media/site-recovery-runbook-automation/03.png)

Seuraavaksi luodaan seuraavat kalusto-tilillä.

### <a name="add-a-subscription-name-as-asset"></a>Lisää resurssi tilauksen nimi

1.  Lisää uusi asetus ![](media/site-recovery-runbook-automation/04.png) Azure automaatio varat ja valitse![](media/site-recovery-runbook-automation/05.png)

2.  Valitse muuttujan **merkkijonona**

3.  Määritä muuttujan nimi **AzureSubscriptionName**

    ![](media/site-recovery-runbook-automation/06.png)

4.  Määritä muuttujan arvon todellinen Azure tilauksen nimi.

    ![](media/site-recovery-runbook-automation/07_1.png)

Saat selville Azure-portaalin tilin asetukset-sivulla tilauksen nimi.

### <a name="add-an-azure-login-credential-as-asset"></a>Lisää resurssi Azure kirjautumisen tunnistetiedot

Azure automaatio käyttää PowerShellin Azure muodostaa tilaus ja palvelutiedot siellä toimii. Tämän ominaisuuden sinun täytyy todentaa käyttäen Microsoft-tili tai työpaikan tai oppilaitoksen tiliä.
Voit tallentaa tilin tunnistetiedot resurssi voidaan käyttää turvallisesti: n runbookin.

1.  Lisää uusi asetus ![](media/site-recovery-runbook-automation/04.png) Azure automaatio varat ja valitse![](media/site-recovery-runbook-automation/09.png)

2.  Valitse **Windows PowerShellin tunnistetiedon** tunnistetiedon tyyppi

3.  Määritä nimi **AzureCredential**

    ![](media/site-recovery-runbook-automation/10.png)

4.  Määritä käyttäjänimi ja salasana, kirjaudu sisään.

Nyt nämä asetukset ovat käytettävissä oman resurssit.

![](media/site-recovery-runbook-automation/11.png)

Lisätietoja yhteyden muodostamisesta tilauksen PowerShellin kautta annetaan [tähän](../powershell-install-configure.md).

Seuraavaksi luodaan runbookin Azure-työkalujen, voit lisätä päätepiste edusta virtuaalikoneen jälkeen automaattisesti.

## <a name="azure-automation-context"></a>Azure automaatio konteksti

ASR välittää konteksti-muuttuja, voit kirjoittaa deterministic komentosarjoja runbookin. Yksi voitu argue pilvipalvelussa ja virtuaalikoneen nimet ovat ennakoitavissa, mutta tapahtuu, ei voi aina kirjainkokoa tietyissä tilanteissa, esimerkiksi sitä, koska jos virtuaalikoneen nimeen on ovat muuttuneet vuoksi virheellisiä merkkejä Azure-tietokannassa. Näin ollen nämä tiedot siirretään ASR palautus suunnitelman osana *kontekstissa*.

Alla on esimerkki kontekstin muuttujan ulkoasun.

        {"RecoveryPlanName":"hrweb-recovery",

        "FailoverType":"Test",

        "FailoverDirection":"PrimaryToSecondary",

        "GroupId":"1",

        "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                {"CloudServiceName":"pod02hrweb-Chicago-test",

                "RoleName":"Fabrikam-Hrweb-frontend-test"}

                }

        }


Seuraavassa taulukossa on kuvattu nimi ja kuvaus muuttujaa kontekstissa.

**Muuttujan nimi** | **Kuvaus**
---|---
RecoveryPlanName | Suunnitelman suoritettavan nimi. Avulla voit toimia käyttämällä saman komentosarjan nimen perusteella
FailoverType | Määrittää, onko testata vikasietotilaa suunniteltu tai suunnittelematon.
FailoverDirection | Määritä, onko palautus ensisijaisen tai toissijaisen
Ryhmätunnus | Tunnistaa palautus-suunnitelma-ryhmän numeron, kun palvelupaketti on käynnissä
VmMap | Matriisin kaikki ryhmän näennäiskoneiden.
VMMap avain | Yksilöivä tunnus (GUID) kunkin AM. Se on sama kuin virtuaalikoneen VMM tunnus tarvittaessa.
RoleName | Nimi, joka palautetaan Azure AM
CloudServiceName | Azure pilvipalvelussa nimi kohdassa virtuaalikoneen luodaan.


Voit tarkistaa VmMap avain kontekstissa voi myös ASR AM ominaisuudet-sivun ja katsomalla AM GUID-ominaisuus.

![](media/site-recovery-runbook-automation/13.png)

## <a name="author-an-automation-runbook"></a>Tekijä automaatio-runbookin

Nyt voit luoda runbookin Avaa edusta virtuaalikoneen 80 porttiin.

1.  Luo uusi runbookin nimellä **OpenPort80** Azure automaatio-tili

    ![](media/site-recovery-runbook-automation/14.png)

2.  Siirry: n runbookin tekijä-näkymä ja kirjoita luonnoksena.

3.  Määritä ensin palautus suunnitelman yhteydessä käytettävä muuttuja

    ```
        param (
            [Object]$RecoveryPlanContext
        )

    ```

4.  Tilaukseesi tunniste- ja tilauksen nimen vieressä yhdistäminen

    ```
        $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

        # Connect to Azure
        $AzureAccount = Add-AzureAccount -Credential $Cred
        $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
        Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
    ```

    Huomaa, että käytät Azure resurssit – **AzureCredential** ja **AzureSubscriptionName** tähän.

5.  Määritä päätepisteen tiedot ja GUID-tunnus, jonka haluat näyttää päätepisteen virtuaalikoneen nyt. Tässä tapauksessa edusta virtuaalikoneen.

    ```
        # Specify the parameters to be used by the script
        $AEProtocol = "TCP"
        $AELocalPort = 80
        $AEPublicPort = 80
        $AEName = "Port 80 for HTTP"
        $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"
    ```

    Määrittää Azure päätepisteen-protokolla, paikallinen porttiin AM ja sen yhdistetyn Julkinen portti. Muuttujia Azure komennot, jotka lisäävät päätepisteet VMs vaatii parametreja. VMGUID pitää haluat käsitellä virtuaalikoneen GUID-tunnus.

6.  Komentosarjan nyt Pura yhteydessä annetun AM GUID ja luoda päätepisteen se viittaa virtuaalikoneen.

    ```
        #Read the VM GUID from the context
        $VM = $RecoveryPlanContext.VmMap.$VMGUID

        if ($VM -ne $null)
        {
            # Invoke pipeline commands within an InlineScript

            $EndpointStatus = InlineScript {
                # Invoke the necessary pipeline commands to add a Azure Endpoint to a specified Virtual Machine
                # Commands include: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including parameters)

                $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                    Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                    Update-AzureVM
                Write-Output $Status
            }
        }
    ```

7. Kun tämä on valmis, Julkaise osumien ![](media/site-recovery-runbook-automation/20.png) sallimaan komentosarjan suorittaminen käytettävissä.

Valmis komentosarja on alla myöhempää käyttöä varten

```
  workflow OpenPort80
  {
    param (
        [Object]$RecoveryPlanContext
    )

    $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

    # Connect to Azure
    $AzureAccount = Add-AzureAccount -Credential $Cred
    $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
    Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

    # Specify the parameters to be used by the script
    $AEProtocol = "TCP"
    $AELocalPort = 80
    $AEPublicPort = 80
    $AEName = "Port 80 for HTTP"
    $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"

    #Read the VM GUID from the context
    $VM = $RecoveryPlanContext.VmMap.$VMGUID

    if ($VM -ne $null)
    {
        # Invoke pipeline commands within an InlineScript

        $EndpointStatus = InlineScript {
            # Invoke the necessary pipeline commands to add an Azure Endpoint to a specified Virtual Machine
            # This set of commands includes: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including necessary parameters)

            $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                Update-AzureVM
            Write-Output $Status
        }
    }
  }
```

## <a name="add-the-script-to-the-recovery-plan"></a>Komentosarjan lisääminen palautus-suunnitelma

Kun komentosarja on valmis, voit lisätä aiemmin luomasi palautus-palvelupakettia.

1.  Valitse luomasi palautus-palvelupaketti, voit lisätä komentosarjan jälkeen ryhmä 2. ![](media/site-recovery-runbook-automation/15.png)

2.  Määritä Komentosarjanimi. Tämä on helppokäyttöinen nimi tämä komentosarjan näyttäminen, palauttaminen suunnitelman kuluessa.

3.  Valitse Azure komentosarjan – vikasietotilaa Azure automaatio tilin nimi.

4.  Valitse Azure-Runbooks olet tekemässä: n runbookin.

![](media/site-recovery-runbook-automation/16.png)

## <a name="primary-side-scripts"></a>Ensisijainen komentosarjoja

Kun suoritetaan automaattisesti Azure, voit valita myös suorittamaan ensisijainen komentosarjoja. Komentosarjat suoritetaan VMM-palvelimen automaattisesti aikana.
Ensisijainen komentosarjoja ovat käytettävissä vain vanhat Sammuta vain ja kirjaa Sammuta vaiheita. Tämä johtuu siitä Odotamme ensisijaisessa ei yleensä ole käytettävissä, kun ilmenee ongelmia.
Vain, jos ensisijainen toimille osallistua se yrittää aikana suunnittelematon automaattisesti, suorita ensisijainen komentosarjoja. Jos ne eivät ole tavoitettavissa tai aikakatkaisu vikasietotilaa säilyvät palauttaa näennäiskoneiden.
Ensisijainen komentosarjoja ovat yhdistelmävisualisoinnin VMware/fyysisiä/Hyper-v sivustojen käytettävissä ilman VMM suojattu Azure - samalla, kun olet Azure automaattisesti.
Kun olet tuntisesta azuren paikalliseen, ensisijainen komentosarjoja (Runbooks) voidaan kaikkien kohteiden VMware lukuun ottamatta.

## <a name="test-the-recovery-plan"></a>Testaa palautus-suunnitelma

Kun olet lisännyt: n runbookin palvelupakettiin, voit aloittaa testi automaattisesti ja katso, miten sitä käytetään. Suositellaan aina Suorita testi automaattisesti, voit testata sovelluksesi ja varmistaa, että ei ole virheitä palautus-suunnitelma.

1.  Palvelupaketti, palauttaminen ja Aloita testi automaattisesti.

2.  Suunnitelman, suorituksen aikana näet onko: n runbookin on suoritettu tai ei sen tila.

    ![](media/site-recovery-runbook-automation/17.png)

3.  Näet myös yksityiskohtaiset runbookin suorittamisen tila: n runbookin Azure Automation työt-sivulla.

    ![](media/site-recovery-runbook-automation/18.png)

4.  Vikasietotilaa päätyttyä, lukuun ottamatta runbookin suorittamisen tulos, näet suorittamisen onnistuu vai ohjesisältöä Azure virtuaalikoneen-sivulle ja päätepisteet katsoo mukaan.

![](media/site-recovery-runbook-automation/19.png)

## <a name="sample-scripts"></a>Komentosarjamallit

Kun Jätimme automatisointi jokin yleisesti käytetyt tehtävä lisätään Azure virtuaalikoneen päätepisteen Tässä opetusohjelmassa, voi tehdä useita muita tehokkaita automaatio tehtävien Azure automaatio. Microsoftin ja Azure automaatio-yhteisön on esimerkki runbooks, jotka auttavat Aloita oman ratkaisuja ja toimintojen runbooks, jossa voit käyttää rakenneosien suurempi automaatio-tehtävien luominen. Aloita ne valikoimasta ja luoda tehokkaita yhdellä napsautuksella palautus suunnitelmien käyttämällä Azure sivuston sovellusten.

## <a name="additional-resources"></a>Lisäresursseja

[Azure automaatio yleiskatsaus] (http://msdn.microsoft.com/library/azure/dn643629.aspx "Azure automaatio yleiskatsaus")

[Azure automaatio-komentosarjamallit] (http://gallery.technet.microsoft.com/scriptcenter/site/search?f[0].Type=User&f[0].Value=SC%20Automation%20Product%20Team&f[0].Text=SC%20Automation%20Product%20Team "Azure automaatio-komentosarjamallit")
