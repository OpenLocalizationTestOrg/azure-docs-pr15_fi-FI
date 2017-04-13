<properties
    pageTitle="Mukautettujen roolien Azure RBAC | Microsoft Azure"
    description="Lue, miten voit määrittää mukautettuja rooleja ja Azure Role-Based käyttöoikeuksien valvonta varten tarkempien jäsenyyksien hallinta Azure-tilaukseesi."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="kgremban"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="07/25/2016"
    ms.author="kgremban"/>


# <a name="custom-roles-in-azure-rbac"></a>Mukautetun Azure RBAC roolit


Luo mukautettu rooli Azure Role-Based Access ohjausobjektin (RBAC), jos mikään valmiit roolit ei vastaa tarpeitasi tietyn access. Mukautettuja rooleja voi luoda käyttämällä [PowerShellin Azure](role-based-access-control-manage-access-powershell.md)ja [REST API](role-based-access-control-manage-access-rest.md) [Azure käyttöliittymä](role-based-access-control-manage-access-azure-cli.md) (CLI). Valmiit roolit, kuten mukautettuja rooleja voi määrittää käyttäjät, ryhmät ja sovellukset on tilaus, resurssiryhmä ja resurssin alueet. Mukautettuja rooleja Azure AD-vuokraajan tallennetaan, ja kaikki tilaukset, jotka käyttävät kyseisen alihallinnan Azure Active directory subsciption voidaan jakaa.

Seuraavassa on esimerkki mukautettu rooli seurantaa ja näennäiskoneiden uudelleen:

```
{
  "Name": "Virtual Machine Operator",
  "Id": "cadb4a5a-4e7a-47be-84db-05cad13b6769",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624",
    "/subscriptions/34370e90-ac4a-4bf9-821f-85eeedeae1a2"
  ]
}
```
## <a name="actions"></a>Toiminnot
Mukautettu rooli **Toiminnot** -ominaisuuden määrittää Azure toimintoja, joihin roolilla antaa käyttöoikeudet. Se on kokoelma toiminnon merkkijonoja, joiden tunnistaa suojattavan toimintojen Azure resurssin palveluista. Toiminto, joka sisältää yleismerkkejä merkkijonot (\*) käyttöoikeus kaikki toiminnot, jotka vastaavat toiminto-merkkijono. Esimerkiksi:

-   `*/read`antaa käyttää lukea kaikki resurssityypit kaikki Azure resurssin tarjoajien toimintoja.
-   `Microsoft.Network/*/read`antaa käyttää lukemaan resurssin kaikenlaisia Microsoft.Network resurssin palvelussa Azure-toimintoja.
-   `Microsoft.Compute/virtualMachines/*`myöntää käytön kaikki toiminnot näennäiskoneiden ja alatason resurssityypit.
-   `Microsoft.Web/sites/restart/Action`antaa käyttää käynnistämään sivustot.

Käytä `Get-AzureRmProviderOperation` (-PowerShell) tai `azure provider operations show` (-Azure CLI) luettelon toiminnot Azure resurssin palveluista. Voi myös käyttää näitä komentoja, voit varmistaa, että toiminto-merkkijono on kelvollinen ja laajenna yleismerkkien toiminnon merkkijonoja.

```
Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT Operation, OperationName

Get-AzureRMProviderOperation Microsoft.Network/*
```

![Näyttökuva PowerShell - Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT toiminnon OperationName](./media/role-based-access-control-configure/1-get-azurermprovideroperation-1.png)

```
azure provider operations show "Microsoft.Compute/virtualMachines/*/action" --js on | jq '.[] | .operation'

azure provider operations show "Microsoft.Network/*"
```

![Azure CLI näyttökuva - azure provider toiminnot Näytä "Microsoft.Compute/virtualMachines/\*/toiminto" ](./media/role-based-access-control-configure/1-azure-provider-operations-show.png)

## <a name="notactions"></a>NotActions
**NotActions** -ominaisuuden avulla, jos toiminnoista, joita haluat sallia joukko helpommin määrittämän rajoitettu toimia lukuun ottamatta. Mukautettu rooli määrittämän käyttöoikeuden lasketaan vähentämällä **Toiminnot** -toimintojen **NotActions** -toimintoja.

> [AZURE.NOTE] Jos käyttäjä on määritetty rooli määrää ulkopuolelle **NotActions**-toiminnon, ja määritetään toinen rooli, joka antaa saman toiminnon käyttöoikeudet, käyttäjä saa tämän toiminnon suorittamiseen. **NotActions** ei estä säännön – se on vain avulla voit luoda joukon sallitut toiminnot tiettyjen toimintojen tarvittaessa jätettävää kätevästi.

## <a name="assignablescopes"></a>AssignableScopes
Mukautetun roolin **AssignableScopes** -ominaisuus määrittää käyttöalueen (tilaukset, resurssiryhmiä tai resursseja), jossa mukautettu rooli on käytettävissä varauksen. Voit määrittää mukautetun roolin vain tilaukset- tai resurssiryhmiä, jotka edellyttävät sitä käytettäväksi varauksen ja tarpeettomat ei käyttäjäkokemuksen muiden tilaukset- tai resurssiryhmiä.

Esimerkkejä kelvollinen voi määrittää alueet:

-   "/ Tilaukset/c276fc76-9cd4-44c9-99a7-4fd71546436e", "/ Tilaukset/e91d47c4-76f3-4271-a796-21b4ecfe3624" - toiminto roolin käyttöön varauksen kaksi-tilauksessa.
-   "/ Tilaukset/c276fc76-9cd4-44c9-99a7-4fd71546436e" - on roolin käytettävissä varauksen yhden tilauksen.
-  "/ Tilaukset/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/verkon" - toiminto rooli varauksen käyttöön vain-verkko-resurssiryhmä.

> [AZURE.NOTE] Käytössä on oltava vähintään yksi tilaus, resurssiryhmä tai resurssin tunnus.

## <a name="custom-roles-access-control"></a>Mukautettuja rooleja käyttää hallinta
Mukautettu rooli **AssignableScopes** -ominaisuuden hallitsee myös, kuka voi tarkastella, muokata ja poistaa roolin.

- Kuka voi luoda mukautettuja roolin?
    Omistajat (ja käyttäjän Access järjestelmänvalvojat) tilauksista, resurssiryhmien ja resurssien voit luoda mukautettuja rooleja käytettäväksi kyseiset alueet.
    Roolin luominen käyttäjän on oltava pysty suorittamaan `Microsoft.Authorization/roleDefinition/write` kaikki **AssignableScopes** toiminnon roolin.

- Ketkä voivat muokata mukautettu rooli?
    Omistajat (ja käyttäjän Access järjestelmänvalvojat) tilauksista, resurssiryhmien ja resurssien voit muokata mukautettuja roolien näiden käyttöalueen. Käyttäjien tarvitsee voi suorittaa `Microsoft.Authorization/roleDefinition/write` kaikki **AssignableScopes** toiminto on mukautettu rooli.

- Kuka voi tarkastella mukautettuja rooleja?
    Valmiin rooleista Azure RBAC rooleihin, jotka ovat käytettävissä varauksen tarkastelun salliminen. Käyttäjät, jotka voit tehdä `Microsoft.Authorization/roleDefinition/read` toiminnon käyttöalue voit tarkastella RBAC rooleihin, jotka ovat käytettävissä resurssivarausten suunnitellut alueen.

## <a name="see-also"></a>Katso myös
- [Roolin perusteella käyttöoikeuksien valvonta](role-based-access-control-configure.md): Aloita RBAC Azure-portaalissa.
- Opi hallitsemaan käytön:
    - [PowerShellin](role-based-access-control-manage-access-powershell.md)
    - [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
    - [REST-OHJELMOINTIRAJAPINNALLA](role-based-access-control-manage-access-rest.md)
- [Valmiin rooleja](role-based-access-built-in-roles.md): roolit, joilla on tullut RBAC-tietoja.
