<properties
   pageTitle="Analysis Services-palvelimen luominen Azure | Microsoft Azure"
   description="Opettele luomaan Analysis Services-palvelimen esiintymä Azure-tietokannassa."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="create-an-analysis-services-server"></a>Analysis Services-palvelimen luominen
Tässä artikkelissa käydään läpi luodaan uusi Analysis Services-palvelimen resurssien Azure-tilaukseesi.

## <a name="before-you-begin"></a>Ennen aloittamista
Aluksi sinun on:

- **Azure tilaus**: Siirry [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/offers/ms-azr-0044p/) ja luo tili.
- **Resurssiryhmä**: resurssiryhmä on jo tai [luoda uuden](../azure-resource-manager/resource-group-overview.md).

> [AZURE.NOTE] Analysis Services-palvelimen luominen saattaa aiheuttaa uuden laskutettavan palvelun. Lisätietoja on artikkelissa Analysis Services hinnoittelua.

## <a name="create-an-analysis-services-server"></a>Analysis Services-palvelimen luominen

1. Kirjautuminen [Azure portal](https://portal.azure.com).

2. Valitse **+ Uusi** > **liiketoimintatietojen + analytics** > **Analysis Services**.

3. **Analysis Services** -sivu-Täytä tarvittavat kentät ja paina sitten **Luo**.

    ![Palvelimen luominen](./media/analysis-services-create-server/aas-create-server-blade.png)

    - **Palvelimen nimi**: Kirjoita yksilöllinen nimi, joka viittaa palvelimeen.

    - **Tilaus**: Valitse tämä palvelin, laskuttaa, asiakasta ensimmäisen kerran tilaus.

    - **Resurssiryhmä**: Nämä ovat säilöjen on suunniteltu helpottamaan Azure resurssien hallinnassa. Lisätietoja on ohjeaiheessa [resurssiryhmistä](../resource-group-overview.md).

    - **Sijainti**: Tämä Azure palvelinkeskuksen sijainnin isännöi palvelimeen. Valitse lähimpään suurin käyttäjän Base-tietokannan sijainti.

    - **Hinnoittelu taso**: Valitse hinnoittelu taso. Taulukko mallit enintään 100 Gigatavua tuetaan. Voit muuttaa hinnoittelu taso myöhemmin.

4. Valitse **Luo**.

Luo yleensä kestää hetken, valitse usein vain muutaman sekunnin. Jos valitsit **Lisää-portaaliin**, siirry portaalin uusi palvelin-kohdassa. Tai Etsi **Lisää palveluja** > **Analysis Services** , jos palvelin on käyttövalmis. Jos se ei näy, Päivitä luettelo.

 ![Raporttinäkymät-ikkunan](./media/analysis-services-create-server/aas-create-server-dashboard.png)


## <a name="next-steps"></a>Seuraavat vaiheet
Kun olet luonut palvelimellesi, voit [ottaa mallin käyttöön](analysis-services-deploy.md) siihen käyttämällä SSDT tai SSMS kanssa.

Jos otat palvelimellesi mallin muodostaa paikallisen tietolähteitä, tarvitset [paikallisen tietojen gateway](analysis-services-gateway.md) asennetaan verkossa olevaan tietokoneeseen.
