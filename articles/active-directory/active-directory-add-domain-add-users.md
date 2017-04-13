<properties
    pageTitle="Käyttäjien määrittäminen mukautetun toimialueen Azure Active Directoryn | Microsoft Azure"
    description="Voit täyttää mukautetun toimialueen Azure Active Directoryn käyttäjätilien."
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="assign-users-to-a-custom-domain"></a>Mukautetun toimialueen käyttäjien määrittäminen

Kun olet lisännyt mukautetun toimialueen Azure Active Directory-käyttäjätileihin toimialueessa on lisättävä niin, että voit aloittaa todennustapa ne.

## <a name="users-synced-in-from-a-directory-on-your-corporate-network"></a>Käyttäjät, jotka on synkronoitu hakemistosta yrityksen verkossa

Jos olet jo määrittänyt yhteyden välillä paikallisen oman Active Directory- ja Azure Active Directory-synkronoinnin voit lisätä tilejä. Saat lisätietoja Azure Active Directory synkronointi paikallisen Active Directory- [integrointi paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).

## <a name="users-added-and-managed-in-the-cloud"></a>Käyttäjät lisätään ja hallita pilveen

Voit muuttaa aiemmin luotua käyttäjätiliä toimialueen seuraavasti:

1.  Avaa Azure perinteinen-portaali tilillä, jolla on yleisen järjestelmänvalvojan tai käyttäjän järjestelmänvalvoja.

2.  Avaa hakemistossa.

3.  Valitse **käyttäjät** -välilehti.

4.  Valitse käyttäjä luettelosta.

5.  Vaihtaa käyttäjän toimialueen, ja valitse sitten **Tallenna**.

Tämä onnistuu myös käyttämällä [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) tai [Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).

## <a name="select-a-custom-domain-when-creating-a-new-user"></a>Valitse mukautetun toimialueen, kun luot uuden käyttäjän

1.  Avaa Azure perinteinen-portaali käyttämällä tiliä, joka on yleisenä järjestelmänvalvojana tai käyttäjän.

2.  Avaa hakemistossa.

3.  Valitse **käyttäjät** -välilehti.

4.  Painikkeita Valitse **Lisää**.

5.  Kun lisäät käyttäjänimi, valitse mukautettu toimialue domain-luettelosta.

6.  Valitse **Tallenna**.

## <a name="next-steps"></a>Seuraavat vaiheet

-   [Käyttämällä mukautettuja toimialuenimiä yksinkertaistaa käyttäjien kirjautuminen](active-directory-add-domain.md)

-   [Mukautetun toimialuenimien hallinta](active-directory-add-manage-domain-names.md)

-   [Lisätietoja domain management käsitteitä Azure AD-](active-directory-add-domain-concepts.md)
