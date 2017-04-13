<properties
    pageTitle="Azure Pinotut (palvelun järjestelmänvalvoja ja vuokraajan) käyttäjää kohti resurssien käyttöoikeuksien hallinta | Microsoft Azure"
    description="Palvelun järjestelmänvalvoja tai vuokraajan tietoja käyttäjäkohtainen resurssien käyttöoikeuksien hallinta."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="manage-user-permissions"></a>Käyttöoikeuksien hallinta

Azure Pinotut käyttäjä voi olla lukija, omistaja tai osallistuja jokaiselle esiintymälle tilauksen, resurssiryhmä tai palveluun. Käyttäjä A voi esimerkiksi reader oikeuksilla tilauksen 1 mutta on omistajan käyttöoikeudet Virtual Machine-7.

-   Reader: Käyttäjä voi tarkastella kaikkea, mutta ei voi tehdä muutoksia.

-   Avustaja: Käyttäjät voivat hallita kaikki muu paitsi resurssien käytön.

-   Omistaja: Käyttäjät voivat hallita kohteiden kaikki, mukaan lukien resurssien käytön.


## <a name="set-access-permissions-for-a-user"></a>Käyttäjän käyttöoikeuksien määrittäminen

1.  Kirjaudu sisään tilille, jolla on haluat hallita resurssien omistajan käyttöoikeuksia.

2.  Napsauta resurssin sivu **Access** -kuvaketta ![](media/azure-stack-manage-permissions/image1.png).

3.  Valitse **käyttäjät** -sivu **roolit**.

4.  Valitse **Lisää** käyttäjän oikeuksien **roolit** -sivu.

## <a name="next-steps"></a>Seuraavat vaiheet

[Lisää Azure pinon vuokraajan](azure-stack-add-new-user-aad.md)
