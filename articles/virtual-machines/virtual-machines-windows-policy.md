<properties
    pageTitle="Käyttää käytäntöjä, Azure Resurssienhallinta näennäiskoneiden | Microsoft Azure"
    description="Voit määrittää käytännön, Azure Resurssienhallinta Windows Virtual Machine"
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
    ms.date="04/13/2016"
    ms.author="singhkay"/>

# <a name="apply-policies-to-azure-resource-manager-virtual-machines"></a>Käyttää käytäntöjä, Azure Resurssienhallinta näennäiskoneiden

Organisaation pakottaa käytännöt käyttämällä eri nimeämiskäytännön ja yrityksen säännöt. Haluttu toiminnan käyttäminen auttaa vähentäminen aikana käytettävät organisaation onnistumista. Tässä artikkelissa on kerrotaan kuinka voi määrittää haluamasi toiminta organisaation näennäiskoneiden Azure Resurssienhallinta käytäntöjen avulla.

Näin suoritettava vaiheet jäsennyksen, joka on alle

1. Azure Resurssienhallinta käytännön 101
2. Käytännön määrittäminen virtuaalikoneen
3. Käytännön luominen
4. Käytä käytäntö

## <a name="azure-resource-manager-policy-101"></a>Azure Resurssienhallinta käytännön 101

Aloittaminen Azure Resurssienhallinta käytännöt suositeltavaa lukemalla artikkelin ohjeiden mukaisesti ja jatkaa sitten tämän artikkelin ohjeita. Artikkelin ohjeiden mukaisesti kuvataan basic määritys ja rakenteen muuttaminen käytännön, kuinka käytännöt Hae lasketaan ja antaa käytännön eri esimerkkejä.

* [Resurssien ja käyttöoikeuksien hallinta käytännön avulla](../resource-manager-policy.md)

## <a name="define-a-policy-for-your-virtual-machine"></a>Käytännön määrittäminen virtuaalikoneen

Yleisiä tilanteita, joissa yrityksen voi olla päästää vain niiden käyttäjien näennäiskoneiden luominen tiettyyn käyttöjärjestelmät, jotka on testattu LOB-sovelluksen kanssa. Azure Resurssienhallinta-käytännön avulla tehtävä voidaan toteuttaa muutaman vaiheen avulla. Käytännön tässä esimerkissä tarkastellaan Salli vain Windows Server 2012 R2 palvelinkeskuksen näennäiskoneiden luodaan. Alla näyttää käytännön määritys

```
"if": {
  "allOf": [
    {
      "field": "type",
      "equals": "Microsoft.Compute/virtualMachines"
    },
    {
      "not": {
        "allOf": [
          {
            "field": "Microsoft.Compute/virtualMachines/imagePublisher",
            "equals": "MicrosoftWindowsServer"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/imageOffer",
            "equals": "WindowsServer"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/imageSku",
            "equals": "2012-R2-Datacenter"
          }
        ]
      }
    }
  ]
},
"then": {
  "effect": "deny"
}
```

Yllä käytäntöä voi muokata helposti tilanne, jossa voit sallia Windows Server palvelinkeskuksen kuva, jota käytetään virtuaalikoneen-ympäristö, jonka alapuolella muuttaminen

```
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*Datacenter"
}
```

#### <a name="virtual-machine-property-fields"></a>Virtuaalikoneen ominaisuuskentät

Seuraavassa taulukossa on kuvattu virtuaalikoneen ominaisuudet, joita voi käyttää kenttinä käytännön määritys. Saat lisätietoja kentät-artikkelin ohjeiden mukaisesti:

* [Kentät ja lähteistä](../resource-manager-policy.md#fields-and-sources)


| Kenttänimi     | Kuvaus                                        |
|----------------|----------------------------------------------------|
| imagePublisher | Määrittää kuvan Publisherissa               |
| imageOffer     | Määrittää valittu kuva Publisherin tarjous |
| imageSku       | Määrittää valitun tarjouksen SKU             |
| imageVersion   | Määrittää valitun SKU Kuvaversio     |

## <a name="create-the-policy"></a>Käytännön luominen

Käytännön voi luoda helposti suoraan REST-Ohjelmointirajapinnalla tai PowerShell cmdlet-komentoja. Käytännön luomiseen on artikkelin ohjeiden mukaisesti:

* [Käytännön luominen](../resource-manager-policy.md#creating-a-policy)


## <a name="apply-the-policy"></a>Käytä käytäntö

Kun olet luonut käytännön sinun on asentaminen määritetyn alueen. Laajuus voi olla tilauksen, resurssiryhmä tai jopa resurssin. Muotoilun käytäntö on artikkelin ohjeiden mukaisesti:

* [Käytännön luominen](../resource-manager-policy.md#applying-a-policy)
