<properties 
   pageTitle="Azure automaatio Runbookin tyypit"
   description="Tässä artikkelissa kuvataan erityyppiset runbooks, jota voit käyttää Azure automaatio ja huomioon otettavia seikkoja, jotka on otettava huomioon määritettäessä apua projektityypin valinnassa. "
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

# <a name="azure-automation-runbook-types"></a>Azure automaatio runbookin tyypit

Azure automaatio tukee neljänlaisia runbooks, joka on kuvattu lyhyesti seuraavassa taulukossa.  Seuraavissa osissa on lisätietoja kunkin tyypin mukaan lukien siitä, milloin kutakin asiat.


| Tyyppi |  Kuvaus |
|:---|:---|
| [Graafinen](#graphical-runbooks) | Windows PowerShellin ja luodaan ja muokattu kokonaan-graafinen editorin Azure-portaalissa perusteella. | 
| [Graafisen PowerShell-työnkulku](#graphical-runbooks) | Windows PowerShellin työnkulun perusteella ja luoda ja muokata kokonaan graafinen editorin Azure-portaalissa. 
| [PowerShellin](#powershell-runbooks) | Tekstin runbookin Windows PowerShell-komentosarjaa perusteella.
| [PowerShell-työnkulku](#powershell-workflow-runbooks) | Tekstin runbookin Windows PowerShellin työnkulun perusteella. |


## <a name="graphical-runbooks"></a>Graafisen runbooks

[Graafiset](automation-runbook-types.md#graphical-runbooks) ja graafiset PowerShell-työnkulun runbooks luodaan ja muokata graafisia editorin Azure-portaalissa.  Voit viedä yhteystiedot tiedostoon ja tuoda ne toiseen automaatio-tiliin, mutta et voi luoda tai muokata niitä jossakin toisessa ohjelmassa.  Graafisen runbooks Luo PowerShell-koodi, mutta ei voi tarkastella tai muokata koodia suoraan. Graafisen runbooks ei voi muuntaa [tekstimuodossa](automation-runbook-types.md)eikä graafisessa muodossa voidaan muuntaa tekstin runbookin. Graafisen runbooks voidaan muuntaa graafinen PowerShell-työnkulun runbooks aikana tuominen ja päinvastoin.

### <a name="advantages"></a>Hyvät puolet

- Luo runbooks mahdollisimman vähän kokemusta [PowerShell](automation-powershell-workflow.md).
- Visuaalisesti prosessit.
- Lisää muita runbooks lapsen runbooks korkean tason työnkulkujen luominen.


### <a name="limitations"></a>Rajoitukset

- Ei voi muokata runbookin Azure portal ulkopuolella.
- Koodi-tehtävän suorittamiseen monitasoista logiikkaa PowerShell-koodia sisältävän saattaa edellyttää.
- Voi tarkastella tai muokata suoraan PowerShell-koodi, joka on luotu graafinen työnkulun. Huomaa, että voit tarkastella koodia, voit luoda koodin tehtävät.


## <a name="powershell-runbooks"></a>PowerShellin runbooks

PowerShellin runbooks perustuvat Windows PowerShell.  Voit muokata suoraan tekstieditorissa Azure-portaalissa runbookin koodi.  Voit käyttää myös minkä tahansa offline-tilassa tekstieditorissa ja [Tuo: n runbookin](http://msdn.microsoft.com/library/azure/dn643637.aspx) Azure automaatio kyselyjä.

### <a name="advantages"></a>Hyvät puolet

- Ota käyttöön kaikki monimutkaisia logiikan ilman muita monimutkaisia PowerShell työnkulun PowerShell-koodilla. 
- Runbookin käynnistyy nopeammin graafiset tai PowerShell työnkulun runbooks, koska sitä ei tarvitse käännettävä ennen sen suorittamista.

### <a name="limitations"></a>Rajoitukset

- On oltava kokemusta PowerShell-komentosarjoja.
- [Rinnakkaisia käsittely](automation-powershell-workflow.md#parallel-processing) ei voi käyttää useita toimintojen suorittamiseen rinnakkain.
- [Tarkistuspisteet](automation-powershell-workflow.md#checkpoints) ei voi käyttää, kun haluat jatkaa runbookin virheen sattuessa.
- PowerShell-työnkulun runbooks ja graafiset runbooks voi olla vain mainittu lapsen runbooks joka luo uuden projektin alkamis-AzureAutomationRunbook cmdlet-komennolla.

### <a name="known-issues"></a>Tunnetut ongelmat
Seuraavassa on PowerShell runbooks tunnetut ongelmat.

- PowerShellin runbooks voi ei voi hakea salaamaton [muuttujan kohteiden](automation-variables.md) kanssa null-arvoa.
- PowerShellin runbooks ei voi hakea [muuttujan kohteiden](automation-variables.md) kanssa *~* nimessä.
- Get-prosessi PowerShellin silmukassa runbookin saattaa kaatua noin 80 iteroinnin jälkeen. 
- PowerShell-runbookin saattaa epäonnistua, jos se yrittää kirjoittaa hyvin suuria määriä tietoja kerralla tulostus-muodossa.   Voit ratkaista ongelman yleensä määritetty vain tiedot, joita tarvitset käsitellessäsi suuria objekteja.  Esimerkiksi sijaan määritetty suunnilleen *Get-prosessi*, voit tulostaa vain tarvittavat kentät, joilla on *Get-prosessin | Valitse prosessinimi suorittimen*.

## <a name="powershell-workflow-runbooks"></a>PowerShell-työnkulun runbooks

PowerShell-työnkulun runbooks ovat teksti runbooks [Windows PowerShellin työnkulun](automation-powershell-workflow.md)perusteella.  Voit muokata suoraan tekstieditorissa Azure-portaalissa runbookin koodi.  Voit käyttää myös minkä tahansa offline-tilassa tekstieditorissa ja [Tuo: n runbookin](http://msdn.microsoft.com/library/azure/dn643637.aspx) Azure automaatio kyselyjä.

### <a name="advantages"></a>Hyvät puolet

- Ota käyttöön kaikki monimutkaisia logiikan PowerShell työnkulun koodilla.
- [Tarkistuspisteet](automation-powershell-workflow.md#checkpoints) avulla voit jatkaa runbookin virheen sattuessa.
- [Rinnakkaisia käsittely](automation-powershell-workflow.md#parallel-processing) avulla voit suorittaa useita toimintoja rinnakkain.
- Voit lisätä muita graafinen runbooks ja PowerShell työnkulun runbooks lapsen runbooks korkean tason työnkulkujen luominen.


### <a name="limitations"></a>Rajoitukset

- Tekijä on oltava kokemusta PowerShell työnkulun.
- Runbookin on käsitellä muita monimutkaisuutta PowerShell työnkulun esimerkiksi [poistaa objekteja](automation-powershell-workflow.md#code-changes).
- Runbookin kestää kauemmin Käynnistä kuin PowerShell runbooks, koska se on käännetty ennen sen suorittamista.
- PowerShellin runbooks voi olla vain mainittu lapsen runbooks joka luo uuden projektin alkamis-AzureAutomationRunbook cmdlet-komennolla.


## <a name="considerations"></a>Huomioon otettavia seikkoja

Sinun on otettava huomioon seuraavat muita huomioon otettavia seikkoja määritettäessä apua projektityypin valinnassa tietyn runbookin varten.

- Et voi muuntaa runbooks-graafinen kirjoita tekstiä tai päinvastoin.
- On käyttäen runbooks erityyppisiä lapsen runbookin rajoituksia.  Lisätietoja on kohdassa [lapsen runbooks Azure automaatio](automation-child-runbooks.md) .

  
## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja graafinen runbookin yhtä aikaa muiden kanssa on artikkelissa [graafiset yhteiskäyttö Azure automaatio](automation-graphical-authoring-intro.md)
- Ymmärtää PowerShell ja PowerShell erot työnkulut runbooks, katso [Learning Windows PowerShell-työnkulku](automation-powershell-workflow.md)
- Katso lisätietoja siitä, miten voit luoda tai tuoda Runbookin [luominen tai Runbookin tuominen](automation-creating-importing-runbook.md)



