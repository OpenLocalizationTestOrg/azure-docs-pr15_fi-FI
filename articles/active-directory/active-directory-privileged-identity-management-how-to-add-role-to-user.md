<properties
   pageTitle="Voit lisätä tai poistaa käyttäjärooliin | Microsoft Azure"
   description="Opi lisää roolit sellaisten käyttäjätietojen Azure Active Directory sellaisten jäsenyyksien hallinta-sovelluksen kanssa."
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
   ms.date="10/24/2016"
   ms.author="kgremban"/>

# <a name="azure-ad-privileged-identity-management-how-to-add-or-remove-a-user-role"></a>Azure AD sellaisten jäsenyyksien hallinta: Voit lisätä tai poistaa käyttäjärooli

Ja Azure Active Directory (AD), Yleinen järjestelmänvalvoja (tai yrityksen järjestelmänvalvoja) päivittää käyttäjät, jotka ovat **pysyvästi** Azure AD-rooleille määritettyjen. Tämä tehdään PowerShellin cmdlet-komennot, `Add-MsolRoleMember` ja `Remove-MsolRoleMember`. Tai he voivat käyttää Azure perinteinen portaalin kuvattuja [Azure Active Directoryn järjestelmänvalvojan roolien määrittäminen](active-directory-assign-admin-roles.md).

Azure AD sellaisten jäsenyyksien hallinta-sovelluksen avulla sellaisten roolin järjestelmänvalvojat voivat tehdä pysyvä roolimäärityksiä, sekä. Lisäksi sellaisten roolin järjestelmänvalvojat voivat tehdä käyttäjien **olevalla** järjestelmänvalvojan roolit. Olevalla järjestelmänvalvoja voit aktivoida rooli, kun käyttäjät tarvitsevat se ja valitse käyttäjien käyttöoikeuksien päättyy, kun olet valmis, ne.

## <a name="manage-roles-with-pim-in-the-azure-portal"></a>Azure-portaalissa PIM ja roolien hallinta

Organisaatiokaavion voit määrittää Azure AD-Office 365: n ja muihin Microsoft-palveluihin ja sovelluksia käyttäjille eri järjestelmänvalvojan roolit.  Käytettävissä olevat roolit lisätietoja tukikäytännöistä [Azure AD PIM roolit](active-directory-privileged-identity-management-roles.md).

Voit lisätä tai poistaa käyttäjän käytössä sellaisten käyttäjätietojen hallinta-roolin, Avaa PIM Raporttinäkymät-ikkunan. Valitse joko **käyttäjien järjestelmänvalvojan roolin** -painiketta tai valitse roolit-taulukon tietyn roolin (kuten yleisenä järjestelmänvalvojana).

> [AZURE.NOTE] Jos et ole ottanut PIM Azure-portaalissa vielä, siirry [Azure AD PIM aloittaminen](active-directory-privileged-identity-management-getting-started.md) tiedot.

Jos haluat antaa toisen käyttäjän PIM itse, jossa PIM edellyttää, että käyttäjällä on roolit ovat edelleen kuvattu [Voit antaa käyttöoikeuden PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="add-a-user-to-a-role"></a>Käyttäjän lisääminen rooli

1. Valitse [Azure portal](https://portal.azure.com/)koontinäytössä **Azure AD sellaisten jäsenyyksien hallinta** -ruutu.
2. Valitse **Hallitse sellaisten rooleja**.
3. **Rooli yhteenveto** -taulukon haluat hallita roolin valitseminen
4. Valitse **Lisää**rooli-sivu.
5. Valitse **Valitse käyttäjät** ja Etsi käyttäjä, **Valitse käyttäjät** -sivu.  
6. Valitse hakutulosten luettelosta käyttäjä ja sitten **Valmis**.
4. Valitse **OK** ja Tallenna valinta. On valittuna käyttäjä näkyy luettelossa tilassa oikeuta rooli.

> [AZURE.NOTE]
>Uusien käyttäjien rooli soveltuvat vain oletusarvoisesti rooli. Jos haluat tehdä rooli pysyvästi, valitse luettelosta käyttäjä. Käyttäjän tiedot tulevat näkyviin uusi sivu. Valitse **Tee oikeudet** käyttäjän tiedot-valikko.  
>Jos käyttäjä ei voi rekisteröidä Azure multi-factor Authentication (MFA) tai käyttää Microsoft-tiliä (yleensä @outlook.com), tarvitset, jotta ne pysyvät niiden roolilla. Rekisteröidy MFA aktivoinnin aikana pyytää olevalla järjestelmänvalvojat.

Nyt kun käyttäjä on oikeutettu rooli, ilmoita siitä, että ne voi aktivoida [aktivoiminen ja aktivoinnin poistamisesta rooli](active-directory-privileged-identity-management-how-to-activate-role.md)ohjeiden mukaisesti.

## <a name="remove-a-user-from-a-role"></a>Käyttäjän poistaminen roolista

Käyttäjien poistaminen olevalla roolimäärityksiä, mutta varmista, että tehtäviä on aina oltava vähintään yksi käyttäjä, joka on pysyvä Yleinen järjestelmänvalvoja.

Voit poistaa tietyn käyttäjän roolia seuraavasti:

1. Siirry rooli rooli luettelosta valitsemalla rooli Azure AD PIM Raporttinäkymät-ikkunan tai valitsemalla **käyttäjien järjestelmänvalvojan roolin** -painiketta.
2. Valitse käyttäjä käyttäjä-luettelosta.
3. Valitse **Poista**. Viesti pyytää sinua vahvistamaan.
4. Valitse **Kyllä** poistaminen käyttäjän rooli.

Jos et ole varma, mitä käyttäjät tarvitsevat silti niiden roolimäärityksiä, valitse voit [käynnistää access-Tarkista oikeuksia](active-directory-privileged-identity-management-how-to-start-security-review.md).


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Seuraavat vaiheet
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
