<properties services="virtual-machines" title="Using Azure CLI with Azure Resource Manager" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machine"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="linux"
   ms.workload="infrastructure"
   ms.date="04/13/2015"
   ms.author="rasquill" />

## <a name="using-azure-cli-with-azure-resource-manager-arm"></a>Azure CLI kanssa Azure resurssien hallinnan avulla (ARM)

Voit käyttää Azure-CLI Resurssienhallinta komennot ja käyttäminen ottamaan Azure resurssien ja Resurssiryhmien käyttäminen työmääriä, sinun on ensin Azure tili (tietenkin). Jos sinulla ei ole tiliä, saat [Azure maksuton tähän](https://azure.microsoft.com/pricing/free-trial/).

> [AZURE.NOTE] Jos sinulla ei vielä ole Azure-tili, mutta sinulla on MSDN-tilaukseen, saat ilmaisia Azure hyvitykset aktivoimalla [MSDN tilaajan edut tähän](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) --tai vapaan tiliä voi käyttää. Joko toimii Azure käytön.

### <a name="step-1-verify-the-azure-cli-version"></a>Vaihe 1: Azure CLI-version tarkastaminen

Käyttämään Azure CLI välttämättömien komentojen ja ARM-mallit, sinun on oltava vähintään versiota 0.8.17. Voit tarkistaa version, kirjoita `azure --version`. Näyttöön tulee suunnilleen:

    $ azure --version
    0.8.17 (node: 0.10.25)

Jos haluat päivittää Azure CLI on artikkelissa [Azure CLI](https://github.com/Azure/azure-xplat-cli).

### <a name="step-2-verify-you-are-using-a-work-or-school-identity-with-azure"></a>Vaihe 2: Tarkista on käytössä on työpaikan tai oppilaitoksen Azure identity

Voit käyttää ARM-komento-tilassa vain, jos käytössäsi on [Azure Active Directory-vuokraajan](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) tai [Palvelun täydellinen käyttäjätunnus](https://msdn.microsoft.com/library/azure/dn132633.aspx). (Näitä kutsutaan myös *organisaation tunnukset*.)

Jos haluat nähdä, onko jokin, kirjaudu sisään kirjoittamalla `azure login` ja käyttämällä työpaikan tai koulun käyttäjänimi ja salasana pyydettäessä. Jos käytössäsi on jokin, katso seuraavanlaiselta:

    $ azure login
    info:    Executing command login
    warn:    Please note that currently you can login only via Microsoft organizational account or service principal. For instructions on how to set them up, please read http://aka.ms/Dhf67j.
    Username: ahmet@contoso.onmicrosoft.com
    Password: *********
  	|info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Setting subscription Visual Studio Ultimate with MSDN as default
    info:    Added subscription Azure Free Trial
    +
    info:    login command OK

Jos tämä ei ole näkyvissä, sinun on luotava Microsoft-tilin tunnistetiedot uuden vuokraajan (tai palvelun pääasiallista). (Tämä on usein henkilökohtaisten MSDN-tilausten tai vapaan kokeilutilaukset kirjoitusmuotoon.) Voit luoda on työpaikan tai oppilaitoksen tunnus, joka on luotu käyttämällä Microsoft-tunnuksen Azure-tililtä, katso [liittää Azure AD-kansio ja uusi Azure-tilaus](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). Jos organisaation tunnus pitäisi olla jo, joudut ehkä puhua tilin puolestasi luoneen henkilön kanssa.

### <a name="step-3-choose-your-azure-subscription"></a>Vaihe 3: Valitse Azure tilauksesi

Jos käytössäsi on vain yksi tilaus Azure-tili, Azure CLI liittää itse oletusarvoisesti kyseisen tilauksen. Jos sinulla on useita tilauksia, sinun on tilaus, jota haluat käyttää kirjoittamalla `azure account set <subscription id or name> true` missä _tilauksen tunnus tai nimi_ on tilauksen tunnus tai tilauksen nimi, jota haluat käsitellä nykyisessä istunnossa.

Näyttöön tulee seuraavanlaiselta:

    $ azure account set "Azure Free Trial" true
    info:    Executing command account set
    info:    Setting subscription to "Azure Free Trial" with id "2lskd82-434-4730-9df9-akd83lsk92sa".
    info:    Changes saved
    info:    account set command OK

### <a name="step-4-place-your-azure-cli-in-the-arm-mode"></a>Vaihe 4: Vie Azure CLI ARM-tilassa

Jos haluat käyttää Azure-CLI Azure resurssien hallinta (ARM)-tilassa, kirjoita `azure config mode arm`. Näyttöön tulee seuraavanlaiselta:

    $ azure config mode arm
    info:    New mode is arm

> [AZURE.NOTE] Voit siirtyä takaisin käyttää Azure service management komentoja kirjoittamalla `azure config mode asm`.
