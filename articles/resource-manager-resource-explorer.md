<properties
   pageTitle="Azure resurssin Explorer | Microsoft Azure"
   description="Tässä artikkelissa kuvataan Azure resurssin Explorer ja kuinka sen avulla voidaan tarkastella ja päivittää ominaisuuksissa Azure Resurssienhallinta kautta"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="stuartleeks"
   manager="ankodu"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/01/2016"
   ms.author="stuartle;tomfitz"/>

# <a name="use-azure-resource-explorer-to-view-and-modify-resources"></a>Azure resurssin Resurssienhallinnan avulla voit tarkastella ja muokata resursseja
[Azure resurssin Explorer](https://resources.azure.com) on erinomainen työkalu katsoo resurssit, jotka olet jo luonut-tilaukseesi. Tämän työkalun avulla tietoja siitä, miten resurssit selkeä rakenne ja nähdä kullekin resurssille varatun ominaisuudet. Tutustu REST API-toiminnot ja PowerShellin cmdlet-komennot, jotka ovat käytettävissä resurssilaji ja lähetät komennot-liittymän kautta. Resurssin Explorer voi olla hyödyllistä, kun olet luomassa Resurssienhallinta mallit, koska sen avulla voit tarkastella aiemmin luotuja resurssien ominaisuudet.

Resurssin Explorer-työkalun lähde on käytettävissä [github](https://github.com/projectkudu/ARMExplorer), joka sisältää arvokkaita viittauksen, jos tarvitset toteuttavien samantyyppinen tilanne Omat sovellukset.

## <a name="view-resources"></a>Näytä resurssit
Siirry [https://resources.azure.com](https://resources.azure.com) ja kirjaudu sisään [Azure Portal](https://portal.azure.com)käyttäisi samoilla tunnuksilla.

Lataamisen, treeview vasemmalla avulla voit siirtyä tilaukset ja resurssiryhmät alirakenteeseen:

![TreeView](./media/resource-manager-resource-explorer/are-01-treeview.png)

Kun resurssiryhmä siirtyminen, jossa on resurssien kyseisen ryhmän tarjoajat tulee näkyviin:

![toimittajien](./media/resource-manager-resource-explorer/are-02-treeview-providers.png)

Sieltä voit aloittaa siirtyminen huomioon resurssin esiintymät. Alla olevassa näyttökuvassa näet `sltest` treeview SQL Server-esiintymän. Oikealla puolella näet REST API pyynnöt, voit käyttää resurssin tietoja. Siirtymällä resurssin solmun resurssi Explorer tekee automaattisesti GET-pyynnössä hakemaan tietoa resurssista. Suuri tekstialueen alapuolella URL-osoite näet Ohjelmointirajapinnan vastausta. 

Kun tutustut Resurssienhallinta malleja sivuston pääsisältöä alkaa tarkistettava tuttuja! Vastauksen **Ominaisuudet** -osan arvoja vastaavia arvoja, voit kirjoittaa mallin **Ominaisuudet** -osan.

![SQL server](./media/resource-manager-resource-explorer/are-03-sqlserver-with-response.png)

Resurssin Resurssienhallinnan avulla voit pitää porautuminen lapsen resursseista kyseessä SQL-tietokantaan, on Aliobjektin asioita, kuten palomuurisäännöt ja resurssit.

Tutustuminen tietokannan näet us tietokannan ominaisuudet. Alla olevassa näyttökuvassa näkyvissä, tietokannan `edition` on `Standard` ja `serviceLevelObjective` (tai tietokannan taso) on `S1`.

![SQL-tietokantaan](./media/resource-manager-resource-explorer/are-04-database-get.png)

## <a name="change-resources"></a>Muuta resursseja

Kun olet siirtynyt resurssille, voit valita Muokkaa-painiketta, jotta JSON-sisältöä voi muokata. Voit muokata JSON ja Lähetä pyyntö HYLLYTETTY muuttaa resurssia sitten resurssin Resurssienhallinnan avulla. Esimerkiksi alla olevassa kuvassa näkyy muuttaa tietokannan taso `S0`:

![tietokannan - HYLLYTETTY pyyntö](./media/resource-manager-resource-explorer/are-05-database-put.png)

**SIJOITA** valitsemalla Lähetä pyyntö. 

Kun kutsu on lähetetty resurssin Explorerin ongelmien uudelleen GET-pyynnössä Päivitä tila. Tässä tapauksessa näemme, joka `requestedServiceObjectiveId` on päivitetty ja eroaa `currentServiceObjectiveId` , joka ilmaisee, skaalauksen toiminto on käynnissä. Voit valita Päivitä tila manuaalisesti GET-painiketta.

![tietokannan – HAE request2](./media/resource-manager-resource-explorer/are-06-database-get2.png)

## <a name="performing-actions-on-resources"></a>Toimintojen tekemistä resursseja

**Toiminnot** -välilehti avulla voit tarkastella ja suorittaa muita muille käyttäjille. Kun olet valinnut sivuston resurssin, Toiminnot-välilehti näkyy käytettävissä olevat toiminnot-osa, jonka alla luettelo on pitkä.

![Web - kirjaa pyyntö](./media/resource-manager-resource-explorer/are-web-post.png)

## <a name="invoking-the-api-via-powershell"></a>Käynnistettäessä Ohjelmointirajapinnan PowerShellin kautta
Resurssin Explorerissa PowerShell-välilehdessä näkyy avulla voit käsitellä resurssi, joka on tällä hetkellä tutustuminen cmdlet-komennot. Olet valinnut resurssin tyypin mukaan näytössä PowerShell-komentosarjaa voi olla yksinkertainen cmdlet-komennot (kuten `Get-AzureRmResource` ja `Set-AzureRmResource`), monimutkaisempia cmdlet-komennot (kuten vaihtaminen paikkojen verkkosivustolla). 

![PowerShellin](./media/resource-manager-resource-explorer/are-07-powershell.png)

Lisätietoja Azure PowerShell cmdlet-komennoista on artikkelissa [Azure PowerShellin Azure resurssien hallinta](powershell-azure-resource-manager.md)

## <a name="summary"></a>Yhteenveto
Resurssien hallinnan työskenneltäessä resurssin Explorer voi olla erittäin hyödyllinen työkalu. On erinomainen tapa löytää tapoja PowerShellin avulla kyselyn ja tehdä siihen muutoksia. Jos työskentelet REST-ohjelmointirajapinnalla on erinomainen tapa aloittaminen ja testaa API-kutsujen nopeasti, ennen kuin aloitat kirjoittamisen koodi. Ja jos kirjoitat malleja voi olla ymmärtää resurssin hierarkian ja Etsi määritys - sijoituspaikka ansiosta voit muutosten tekeminen portaalissa ja Etsi vastaavat kohdat resurssin Explorerissa!

Lisätietoja on Katso [video Scott Hanselman ja David Ebbo kanavan 9](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Resource-Manager-Explorer-with-David-Ebbo)


