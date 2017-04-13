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


# <a name="add-azure-automation-runbooks-to-recovery-plans"></a>Lisää Azure automaatio runbooks palautus-Palvelupaketit


Tässä opetusohjelmassa kuvataan, miten Azure palauttaminen integroituu Azure automaatio antamaan laajennettavuus palautus-palvelupaketin. Palautus suunnitelmien voit orchestrate oman näennäiskoneiden suojattu käyttäen Azure palauttaminen toissijainen pilven replikointi ja Azure skenaariot replikoinnin palauttaminen. Ne helpottavat myös tekemistä esimerkiksi palautus **johdonmukaisesti tarkkoja** **toistettavien**ja **Automaattinen**. Jos epäonnistui tuntemattomasta Azure, että näennäiskoneiden päälle, Azure automaatio integrointi laajentaa palautus-suunnitelmien ja antaa ominaisuuksien runbooks, jolloin tehokkaita automaatio tehtävien suorittamiseen.

Jos olet ei kuullut Azure automaatio vielä, Rekisteröidy [tähän](https://azure.microsoft.com/services/automation/) ja lataa niiden komentosarjamallit [tähän](https://azure.microsoft.com/documentation/scripts/). Lue lisätietoja [Azure palauttaminen](https://azure.microsoft.com/services/site-recovery/) ja niiden orchestrate palauttaminen käyttämällä palautus suunnitelmien Azure [tähän](https://azure.microsoft.com/blog/?p=166264).

Tässä opetusohjelmassa on näyttää, miten voit integroida Azure automaatio runbooks palautus suunnitelmien. Yksinkertainen, joka vaaditaan aiemmin manuaalinen toimia tehtävien automatisointi ja katso, miten voit muuntaa monivaiheinen palautuksen yhdellä napsautuksella palautus-toiminto. On myös näyttää, miten voit vianmääritys yksinkertaisen, jos se vikaan.

## <a name="customize-the-recovery-plan"></a>Mukauta palautus-suunnitelma

1. Anna meidän Aloita operning palautus-palvelupaketin resurssi-sivu. Voit tarkastella palautus-suunnitelmassa on kaksi näennäiskoneiden on lisätty palauttamista varten. 

    ![](media/site-recovery-runbook-automation-new/essentials-rp.PNG)
---------------------

2. Valitse Aloita lisäämällä runbookin Mukauta-painiketta. Tämä avaa palautus-suunnitelma mukauttaminen sivu.


    ![](media/site-recovery-runbook-automation-new/customize-rp.PNG)


3. Napsauta hiiren kakkospainikkeella Käynnistä-ryhmä 1 ja valitse Lisää "Lisää Kirjaa toiminto".

4. Valitse tämä vaihtoehto, jos haluat valita komentosarjan uusi sivu.

5. Nimeä komentosarja "Hei-maailman".

6. Valitse Automaattiset asiakkaan nimi. Tämä on Azure automaatio-tili. Huomaa, että tilin voi olla mikä tahansa Azure geography, mutta se on oltava sama palauttaminen säilö-tilauksen.

7. Valitse runbookin automaatio-tili. Tämä on komentosarja, joka suoritetaan, kun palautus on ensimmäisen ryhmän palauttaminen suunnitelman suorituksen aikana.

    ![](media/site-recovery-runbook-automation-new/update-rp.PNG)


8. Voit tallentaa komentosarja valitsemalla OK. Tämä Lisää komentosarja kirjaa toiminnon käyttäjäryhmään ryhmän 1: Käynnistä-painiketta.


    ![](media/site-recovery-runbook-automation-new/addedscript-rp.PNG)


## <a name="salient-points-of-adding-a-script"></a>Tärkeitä kohtiin komentosarjan lisääminen

1. Voit komentosarja napsauttamalla hiiren kakkospainikkeella ja valita 'Poista vaihe' tai "päivittää komentosarjan".

2. Komentosarjan-paikallisen voidaan suorittaa, kun automaattisesti Azure Azure ja voi suorittaa Azure ensisijainen komentosarja Sammuta, ennen kuin paikalliseen tuntisesta azuren aikana.

3. Komentosarjan suoritetaan, kun se Lisää palautus suunnitelman kontekstissa.

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
CloudServiceName (-resurssien hallinnan käyttöönottomalli) | Azure resurssiryhmä nimi kohdassa virtuaalikoneen luodaan.


## <a name="using-complex-variables-per-recovery-plan"></a>Kompleksiluvun kohti palautus suunnitelman muuttujien käyttäminen

Joskus runbookin edellyttää enemmän tietoja kuin vain RecoveryPlanContext. Ei ole ensimmäisen luokan järjestelmä välittää parametri runbookin. Jos haluat käyttää samaa komentosarjan kautta useita palautus-suunnitelmien voit käytetään palautus suunnitteleminen konteksti muuttujan 'RecoveryPlanName' ja alapuolella koe tekniikka Azure automaatio-kompleksiluvun muuttujan käyttämisestä runbookin. Seuraavassa esimerkissä näkyy, miten voit luoda kolme eri kompleksiluvun muuttujan varat ja käyttää niitä runbookin palautus-palvelupaketin nimen perusteella.

Ota huomioon, että haluat käyttää 3 muut parametrit runbookin. Anna meidän koodata JSON lomakkeeseen {"Var1": "testautomation", "Var2": "Suunnittelematon", "Var3": "PrimaryToSecondary"}

Luo uusi automaatio-resurssi [AA kompleksiluvun muuttujan](../automation/automation-variables.md#variable-types Complex variable) avulla.
Nimi kuin muuttujan <RecoveryPlanName>- parametrit.
Voit luoda [monimutkaisia muuttujan](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396)muutettava viittaus.

Eri palautus-palvelupaketti nimi kuin muuttuja

1. recoveryPlanName1 >-Parametrit
2. recoveryPlanName2 >-Parametrit
3. recoveryPlanName3 >-Parametrit

Nyt komentosarjan viitata parametrit

1. Pyydä RP nimi $rpname = $Recoveryplancontext muuttuja
2. Hae $paramValue resurssi = "$($rpname)-parametrit"
3. Tämän toiminnon avulla kompleksiluvun muuttujana palautus suunnitelman soittamalla Get-AzureAutomationVariable [-AutomationAccountName] <String> -$paramValue nimi.

Esimerkkinä saat kompleksiluvun muuttujan/parametrin SharepointApp palautus suunnitelman-luominen Azure automaatio kompleksiluvun muuttujaan, jonka 'Parametrit SharepointApp'.

Käytä palautus-suunnitelmassa vähentämällä muuttuja-lauseella Get-AzureAutomationVariable resurssi [-AutomationAccountName] <String> [-nimi] $paramValue. [Viittaus tämä lisätietoja](https://msdn.microsoft.com/library/dn913772.aspx)

Tällä tavalla saman komentosarjan voidaan käyttää eri palautus suunnitelman suunnitelman tietyn kompleksiluvun muuttujan tallentamiseen varoihin.

## <a name="sample-scripts"></a>Komentosarjamallit

Säilön komentosarjojen, voit tuoda suoraan automaatio-tilillesi artikkeleissa [komentosarjojen Kristian kiinalainen OMS säilö](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/Solutions/asrautomation)

Komentosarja tähän on Azure Resurssienhallinta-malli, joka otetaan käyttöön kaikissa-komentosarjoja alapuolella

* NSG

NSG: n runbookin määrittää julkiseen IP-osoitteiden jokaisen AM sisällä palautus suunnittelu- ja niiden virtual verkkosovittimien liittäminen verkon käyttöoikeusryhmän, joka sallii oletusarvon viestintä

* PublicIP

Julkiseen IP-runbookin määrittää julkiseen IP-osoitteiden jokaisen AM sisällä palautus suunnitteleminen. Koneet ja sovellusten käyttöoikeudet riippuu kunkin Vieras palomuuriasetukset


* CustomScript

CustomScript runbookin määrittää julkiseen IP-osoitteiden jokaisen AM sisällä palautus suunnitteleminen ja asenna mukautettu komentosarja-tunniste, joka otetaan mallin käyttöönoton aikana viitata komentosarja

* NSGwithCustomScript

Runbookin liitetään julkiseen IP-osoitteiden jokaisen AM sisällä palautus suunnittelu ja asenna mukautettu komentosarja käyttämällä tunniste ja Yhdistä virtual verkkosovittimien NSG, jolla sallitaan NSGwithCustomScript oletus saapuvan ja lähtevän tietoliikenteen etäkäyttöä varten

## <a name="additional-resources"></a>Lisäresursseja

[Azure Automation Service Suorita tiliksi](../automation/automation-sec-configure-azure-runas-account.md)

[Azure automaatio yleiskatsaus] (http://msdn.microsoft.com/library/azure/dn643629.aspx "Azure automaatio yleiskatsaus")

[Azure automaatio-komentosarjamallit] (http://gallery.technet.microsoft.com/scriptcenter/site/search?f[0].Type=User&f[0].Value=SC%20Automation%20Product%20Team&f[0].Text=SC%20Automation%20Product%20Team "Azure automaatio-komentosarjamallit")
