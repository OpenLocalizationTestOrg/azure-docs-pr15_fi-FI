<properties
    pageTitle="Roolipohjainen käyttöoikeuksien valvonta käyttäminen Azure portaalin | Microsoft Azure"
    description="Aloittaminen: käyttöoikeushallinta Roolipohjainen käyttöoikeuksien valvonta Azure-portaalissa. Käyttöoikeuksien määrittäminen resurssien roolimäärityksiä avulla."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="use-role-assignments-to-manage-access-to-your-azure-subscription-resources"></a>Azure tilaus-resurssien käyttöoikeuksien hallinta roolimäärityksiä avulla

> [AZURE.SELECTOR]
- [Käyttäjän tai ryhmän käyttöoikeuksien hallinta](role-based-access-control-manage-assignments.md)
- [Käyttöoikeuksien hallinta resurssien mukaan](role-based-access-control-configure.md)

Azure Roolipohjainen käytön hallinta (RBAC) mahdollistaa Azure tarkasti rajattuja käyttöoikeushallinta. Käytä RBAC, voit myöntää access määrä, että käyttäjien on suoritettava niiden työt. Tämän artikkelin avulla pääset hyvin alkuun kanssa RBAC Azure-portaalissa. Jos haluat lisätietoja siitä, miten RBAC avulla voit hallita niiden käyttöä, [Roolipohjainen käyttöoikeuksien valvonta on](role-based-access-control-what-is.md)artikkelissa.

## <a name="view-access"></a>Näkymä access
Näet, kenellä on oikeudet resurssi, resurssiryhmä tai tilauksen sen tärkeimmät sivu- [Azure portal](https://portal.azure.com). Esimerkiksi haluamme kenellä on oikeudet johonkin tämän resurssiryhmistä:

1. Valitse **resurssiryhmät** siirtymispalkissa vasemmalla.  
    ![Resurssiryhmät - kuvake](./media/role-based-access-control-configure/resourcegroups_icon.png)
2. Valitse resurssiryhmän nimi **resurssiryhmät** -sivu.
3. Valitse vasemmasta valikosta **käyttöoikeuksien hallinta (IAM)** .  
4. Accessin hallinta-sivu näyttää kaikki käyttäjät, ryhmät ja sovelluksia, jotka on myönnetty käyttöoikeus resurssiryhmä.  

    ![Käyttäjät-sivu - perittyjä ja määritetty access-näyttökuva](./media/role-based-access-control-configure/view-access.png)

Huomaa, että joillakin käyttäjillä on **määritetty** käyttämään samalla, kun muut **peritty** sitä. Access on määritetty erityisesti resurssiryhmän tai varaus periytyvät ylätason-tilaukseen.

> [AZURE.NOTE] Perinteinen tilauksen Järjestelmänvalvojat ja apuyhteyshenkilöiden tiedostoilla omistajat mallissa RBAC tilaus.


## <a name="add-access"></a>Access lisää
Voit myöntää resurssi, resurssiryhmä tai tilauksen, joka on roolimääritys laajuus-käyttö.

1. Valitse **Lisää** käytön hallinta-sivu.  
2. Valitse rooli, jolle haluat määrittää **Valitse rooli** -sivu.
3. Valitse käyttäjän, ryhmän tai sovelluksen hakemistossa, jolle haluat antaa oikeudet. Voit hakea hakemiston näyttönimiä, sähköpostiosoitteet ja tunnukset.  

    ![Lisää käyttäjät-sivu - Etsi näyttökuva](./media/role-based-access-control-configure/grant-access2.png)

4. Valitse **OK** , jos haluat luoda tehtävän. Ponnahduslomakkeen **lisääminen käyttäjän** seuraa edistymistä.  
    ![Lisäämällä käyttäjän tilanneilmaisin - näyttökuva](./media/role-based-access-control-configure/addinguser_popup.png)

Kun olet lisännyt rooli tehtävän onnistuneesti, se näkyy **käyttäjät** -sivu.

## <a name="remove-access"></a>Käyttöoikeuden poistaminen

1. Valitse roolimääritys-ohjausobjektin Access-sivu.
2. Valitse **Poista** tehtävän tiedot-sivu.  
3. Valitse **Kyllä** Vahvista poistaminen.  
    ![Käyttäjät-sivu - roolin näyttökuva poistaminen](./media/role-based-access-control-configure/remove-access1.png)

Peritään tehtäviä ei voi poistaa. Huomaa alla olevassa kuvassa, Poista-painike näkyy harmaana. Tarkista, **Määritetty osoitteessa** yksityiskohtaiset tiedot. Siirry luettelossa voit poistaa roolimääritys resurssi.

![Käyttäjät-sivu - perittyjä access poistaa käytöstä poistaminen painikkeen kuva](./media/role-based-access-control-configure/remove-access2.png)

## <a name="other-tools-to-manage-access"></a>Muita työkaluja käyttöoikeuksien hallinta
Voit määrittää roolit ja hallita niiden käyttöä työkaluja kuin Azure portaalin Azure RBAC komennoilla.  Saat lisätietoja edellytykset ja Azure RBAC komentoja aloittaminen linkeistä.

- [Azure PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure käyttöliittymä](role-based-access-control-manage-access-azure-cli.md)
- [REST-OHJELMOINTIRAJAPINNALLA](role-based-access-control-manage-access-rest.md)

## <a name="next-steps"></a>Seuraavat vaiheet
- [Access-muuta historia-raportin luominen](role-based-access-control-access-change-history-report.md)
- Katso [RBAC valmiit roolit](role-based-access-built-in-roles.md)
- Oma [Mukautettu Azure RBAC roolien](role-based-access-control-custom-roles.md) määrittäminen
