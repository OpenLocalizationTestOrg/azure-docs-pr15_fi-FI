<properties
   pageTitle="Miten voi antaa käyttöoikeuden PIM | Microsoft Azure"
   description="Opettele lisäämään roolit käyttäjille Azure Active Directory sellaisten jäsenyyksien hallinta-tunniste, jotta hän voi hallita PIM."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/22/2016"
   ms.author="kgremban"/>

# <a name="how-to-give-access-to-manage-azure-ad-privileged-identity-management"></a>Miten voi antaa käyttöoikeuden hallittavan Azure AD sellaisten jäsenyyksien hallinta

Yleinen järjestelmänvalvoja, joka mahdollistaa organisaation Azure AD sellaisten käyttäjähallinnan (PIM) automaattisesti Hae roolimäärityksiä ja PIM käyttöoikeudet. Kukaan muu ei saa kirjoitusoikeudet oletusarvoisesti, mukaan lukien muiden Yleiset järjestelmänvalvojat. Muut yleinen järjestelmänvalvojia, suojauksen Järjestelmänvalvojat ja suojauksen lukijat on vain luku-Azure AD PIM käyttöoikeus. Voi antaa käyttöoikeuden PIM, ensimmäinen käyttäjä määrittää muiden **sellaisten roolin** järjestelmänvalvojaksi. Tämän tehtävän on tehtävä PIM itse, eikä niitä voi muuttaa PowerShell tai muita portaaleihin.

> [AZURE.NOTE] Azure MFA hallinta Azure AD PIM edellyttää. Koska Microsoft-tilit et voi rekisteröidä Azure MFA, Microsoft-tilillä kirjautuva käyttäjä ei voi käyttää Azure AD PIM.

Varmista, että on aina vähintään kaksi käyttäjää sellaisten roolin järjestelmänvalvoja-roolia siltä varalta, yksi käyttäjä on lukittu tai niiden tili poistetaan.

## <a name="give-another-user-access-to-manage-pim"></a>Anna toisen käyttöoikeuden PIM hallinta

1. Kirjautuminen [Azure-portaaliin](https://portal.azure.com/) ja valitse koontinäytössä **Azure AD sellaisten jäsenyyksien hallinta** -sovellus.
2. Valitse **Hallitse sellaisten rooleja** > **sellaisten roolin järjestelmänvalvojan** > **Lisää**.

    ![Lisää sellaisten roolin järjestelmänvalvojat - näyttökuva][1]

4. Lisää hallitun käyttäjät-sivu, valitse vaiheessa 1 on jo valmis. Valitse Vaihe 2, **Valitse käyttäjät** ja Etsi käyttäjä, johon haluat lisätä.

    ![Valitse käyttäjät - näyttökuva][2]

6. Valitse käyttäjä hakutuloksista ja sitten **Valmis**.
7. Valitse **OK** ja Tallenna valinta. Olet valinnut käyttäjän näkyvät sellaisten roolin järjestelmänvalvojien luetteloon.

    - Aina, kun määrität uuden roolin muille, ne määritetään automaattisesti kuin oikeutettuja aktivoimaan rooli. Jos haluat, jotta ne pysyvät roolissa, valitse käyttäjä luettelosta. Valitse **Tee oikeudet** käyttäjän tiedot-valikko.

8. Käyttäjän linkin lähettäminen [aloittaminen Azure AD sellaisten tunnistetietojen hallinta](active-directory-privileged-identity-management-getting-started.md).


## <a name="remove-another-users-access-rights-for-managing-pim"></a>Poistaa toisen käyttäjän käyttöoikeuksia PIM hallinta

Ennen kuin poistat jonkun sellaisten roolin järjestelmänvalvojan roolin, varmista aina, käytettävissä on kaksi käyttäjää on määritetty.

1. Napsauta PIM Raporttinäkymät-ikkunan **sellaisten rooli-järjestelmänvalvojan**rooli.  Luettelo käyttäjistä tällä hetkellä roolin näkyvät.
2. Valitse käyttäjä käyttäjä-luettelosta.
3. Valitse **Poista**.  Näyttöön tulee vahvistussanoma.
4. Valitse **Kyllä** voit poistaa käyttäjän roolista.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Seuraavat vaiheet
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_add_PRA.png
[2]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_select_users.png
