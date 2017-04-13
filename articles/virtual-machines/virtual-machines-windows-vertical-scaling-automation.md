<properties
    pageTitle="Skaalaa pystysuunnassa Azuren näennäiskoneiden kanssa Azure automaatio | Microsoft Azure"
    description="Miten pystysuunnassa Windows-Virtual Machine vastauksena valvominen ilmoitusten Azure automaatio kanssa"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/29/2016"
    ms.author="singhkay"/>

# <a name="vertically-scale-azure-virtual-machines-with-azure-automation"></a>Skaalaa pystysuunnassa Azuren näennäiskoneiden Azure automaatio kanssa

Skaalauksen pystysuunnassa käsitellään suurenee tai pienenee tietokoneen työmäärää saatuaan resurssit. Azure-tietokannassa tämä onnistuu muuttamalla virtuaalikoneen kokoa. Tämän ohjeen seuraavissa tilanteissa:

- Jos virtuaalikoneen ei käytetä usein, voit muuttaa sen kokoa pienemmäksi kuukausittain kustannusten alaspäin
- Jos virtuaalikoneen on piikin kuormituksen, se voi kokoa niin, että sen kapasiteetin suurentaminen

Näin suoritettava vaiheet jäsennyksen, joka on alle

1. Voit käyttää omaa näennäiskoneiden automaatio Azure asetukset
2. Tilauksen Azure automaatio pystysuora asteikko-runbooks tuominen
3. Oman runbookin webhook lisääminen
4. Ilmoituksen lisääminen virtuaalikoneen

> [AZURE.NOTE] Ensimmäinen virtuaalikoneen, se voi skaalata, koot koon vuoksi saattaa olla rajoitettu vuoksi käytettävyyttä klusterin muut koot nykyistä on otettu käyttöön. Tässä artikkelissa käytetyt julkaistu automaatio runbooks on tässä tapauksessa huolehtia ja vain skaalata sisällä AM koon paria alapuolella. Tämä tarkoittaa, että Standard_D1v2-Virtual Machine ei yhtäkkiä on sovitettu Standard_G5 tai skaalata Basic_A0.

>| AM kokoa skaalaus pari |   |
|---|---|
|  Basic_A0 |  Basic_A4 |
|  Standard_A0 | Standard_A4 |
|  Standard_A5 | Standard_A7  |
|  Standard_A8 | Standard_A9  |
|  Standard_A10 |  Standard_A11 |
|  Standard_D1 |  Standard_D4 |
|  Standard_D11 | Standard_D14  |
|  Standard_DS1 |  Standard_DS4 |
|  Standard_DS11 | Standard_DS14  |
|  Standard_D1v2 |  Standard_D5v2 |
|  Standard_D11v2 |  Standard_D14v2 |
|  Standard_G1 |  Standard_G5 |
|  Standard_GS1 |  Standard_GS5 |

## <a name="setup-azure-automation-to-access-your-virtual-machines"></a>Voit käyttää omaa näennäiskoneiden automaatio Azure asetukset

Huomaa, sinun on suoritettava ei luoda Azure automaatio-tilin, joka isännöi käytettävä skaalata Virtual Machine runbooks. Viimeksi automaatio-palvelun käyttöön "Suorita tiliksi"-toiminto, jolla on suunniteltu asetus palvelun lyhennys määrittäminen automaattinen suorittaminen runbooks käyttäjän puolesta hyvin helppoa. Lue lisätietoja tämän artikkelin ohjeiden mukaisesti:

* [Todennetaan Runbooks Azure Suorita nimellä-tilillä](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-the-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Tilauksen Azure automaatio pystysuora asteikko-runbooks tuominen

Runbooks, joita tarvitaan skaalauksen pystysuunnassa virtuaalikoneen on jo julkaistu Azure automaatio Runbookin-valikoimassa. Tarvitset tuominen tilauksen. Lue, miten voit tuoda runbooks lukemalla seuraavassa artikkelissa.

* [Azure automatisointiin Runbookin ja moduulin valikoimat](../automation/automation-runbook-gallery.md)

Alla olevassa kuvassa on esitetty runbooks vaikuttavat voi tuoda

![Tuo runbooks](./media/virtual-machines-vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-to-your-runbook"></a>Oman runbookin webhook lisääminen

Kun olet tuonut runbooks, sinun on Lisää webhook: n runbookin, jotta se voidaan käynnistämän ilmoituksen Virtual tietokoneesta. Tietoja oman Runbookin webhook luominen voi lukea tähän

* [Azure automaatio webhooks](../automation/automation-webhooks.md)

Varmista, että voit kopioida webhook ennen kuin suljet webhook-valintaikkuna, kun tarvitset tämä seuraavan osion.

## <a name="add-an-alert-to-your-virtual-machine"></a>Ilmoituksen lisääminen virtuaalikoneen

1. Valitse virtuaalikoneen asetukset
2. Valitse "Ilmoitusten säännöt"
3. Valitse "Lisää ilmoitus"
4. Valitse Kyselysäännön ilmoitus arvo
5. Valitse ehto, joka on täytetty aiheuttaa ilmoituksen Kyselysäännön
6. Valitse vaiheessa 5 ehto raja-arvo. on täytettävä
7. Valitse ehto ja kynnysarvo tulosjoukko, jolle seuranta-palvelu tarkistaa kausi vaiheet 5 ja 6
8. Liitä kopioitu edellisessä osassa webhook.

![Lisää ilmoitus virtuaalikoneen 1](./media/virtual-machines-vertical-scaling-automation/add-alert-webhook-1.png)

![Lisää ilmoitus virtuaalikoneen 2](./media/virtual-machines-vertical-scaling-automation/add-alert-webhook-2.png)
