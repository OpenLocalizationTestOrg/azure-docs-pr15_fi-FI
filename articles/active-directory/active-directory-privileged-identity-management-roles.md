<properties
   pageTitle="Roolien PIM | Microsoft Azure"
   description="Katso, mitä rooleja käytetään sellaisten käyttäjätietojen Azure sellaisten jäsenyyksien hallinta-tunniste."
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
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# <a name="roles-in-azure-ad-privileged-identity-management"></a>Roolien Azure AD-luottamuksellisten jäsenyyksien hallinta

<!-- **PLACEHOLDER: Need description of how this works. Azure PIM uses roles from MSODS objects.**-->

Voit määrittää organisaation eri järjestelmänvalvojan rooleihin Azure AD-käyttäjille. Nämä roolimäärityksiä määrittää, mitkä tehtävät, kuten lisäämällä tai poistamalla käyttäjiä tai palveluasetusten muuttaminen käyttäjät voivat suorittaa Azure AD-Office 365: ssä ja Microsoft Online Services ja sovelluksia.  

Yleinen järjestelmänvalvoja voi päivittää käyttäjät, jotka ovat **pysyvästi** rooleille määritettyjen Azure AD-käyttää PowerShellin cmdlet-komennot, kuten `Add-MsolRoleMember` ja `Remove-MsolRoleMember`, tai palvelun sellaisena kuvatun [järjestelmänvalvojan roolien määrittäminen Azure Active Directory-](active-directory-assign-admin-roles.md)perinteinen-portaalissa.

Azure AD-luottamuksellisten jäsenyyksien hallinta (PIM) hallitsee Azure AD-käytäntöjä sellaisten käyttäjien käyttöoikeuksia. PIM määrittää käyttäjät vähintään yksi rooleille Azure AD- ja voit määrittää joku on pysyvästi, rooli tai oikeuta rooli. Kun käyttäjä määritetään pysyvästi rooli tai Aktivoi olevalla roolimääritys, valitse ne voivat hallita Azure Active Directory, Office 365: ssä ja muissa sovelluksissa niiden rooleille määritettyjen käyttöoikeuksien kanssa.

Ei ole eroja jonkun pysyvästi ja olevalla roolimääritys antaa Accessissa. Ainoa ero on se, että joillakin käyttäjillä ei tarvitse että aina. Tehdään oikeuta rooli ja käynnistä se ja käytöstä aina, kun he tarvitsevat.

## <a name="roles-managed-in-pim"></a>Hallitaan PIM roolit

Sellaisten jäsenyyksien hallinta, voit määrittää käyttäjät yleisiä järjestelmänvalvojan rooleihin liittyvistä aiheista:


- **Yleinen järjestelmänvalvoja** (tunnetaan myös nimellä yrityksen järjestelmänvalvoja) käyttää kaikkia järjestelmänvalvojan ominaisuuksia. Organisaatiossa voi olla useita Yleinen järjestelmänvalvoja. Henkilö, jolla voit ostaa Office 365: n automaattisesti Rekisteröi tulee yleisellä järjestelmänvalvojalla.
- **Sellaisten roolin järjestelmänvalvoja** hallitsee Azure AD PIM ja päivittää roolimäärityksiä muille käyttäjille.  
- **Laskutuksen järjestelmänvalvoja** tekee hankintoja, hallinnoi tilauksia, hallinnoi tukipyyntöjä ja valvoo palvelun kuntoa.
- **Salasanajärjestelmänvalvoja** palauttaa salasana, hallinnoi palvelupyyntöjä ja valvoo palvelun kuntoa. Salasanajärjestelmänvalvojat rajoittuvat salasanojen käyttäjille.
- **Palvelun järjestelmänvalvoja** hallinnoi palvelupyyntöjä ja näyttöjen palvelun kunto.

  > [AZURE.NOTE] Jos käytössäsi on Office 365: ssä, valitse ennen palvelun järjestelmänvalvojan roolin määrittäminen toiselle käyttäjälle, ensin määrittää käyttäjälle järjestelmänvalvojan oikeudet-palveluun, kuten Exchange Online.

- **Käyttäjähallinnan järjestelmänvalvoja** palauttaa salasana, valvoo palvelun kuntoa ja hallitsee käyttäjätilit, käyttäjäryhmät ja palvelupyyntöjä. Käyttäjähallinnan järjestelmänvalvoja et voi yleisen järjestelmänvalvojan poistaminen, Luo muita järjestelmänvalvojan roolia tai Yleinen, Laskutus- ja palvelun järjestelmänvalvojat järjestelmänvalvojan salasanan.
- **Exchange-järjestelmänvalvoja** omistaa järjestelmänvalvojan käyttöoikeuden Exchange Onlineen Exchange-hallintakeskuksen (EAC) kautta ja voi suorittaa melkein mitä tahansa tehtäviä Exchange online-tilassa.
- **SharePoint-järjestelmänvalvoja** omistaa järjestelmänvalvojan käyttöoikeuden SharePoint Onlineen SharePoint Online-hallintakeskuksen kautta ja voi suorittaa melkein mitä tahansa tehtäviä SharePoint Onlinessa.
- **Skype for Business-järjestelmänvalvoja** omistaa järjestelmänvalvojan käyttöoikeuden Skype for Businessiin Skype for Business-hallintakeskuksen kautta ja voi suorittaa melkein mitä tahansa tehtäviä Skype for Business Online.

Seuraavat artikkelit lisätietoja [Azure AD määrittää järjestelmänvalvojan roolit](active-directory-assign-admin-roles.md) ja [järjestelmänvalvojan roolien määrittäminen Office 365: ssä](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504).

<!--**PLACEHOLDER: The above article may not be the one we want since PIM gets roles from places other that Office 365**-->


Valitse PIM voit [määrittää käyttäjälle rooli](active-directory-privileged-identity-management-how-to-add-role-to-user.md) niin, että käyttäjä voi [aktivoida roolin tarvittaessa](active-directory-privileged-identity-management-how-to-activate-role.md).

Jos haluat antaa toisen käyttäjän käyttää PIM itse hallinnassa, joka PIM edellyttää, että käyttäjällä on roolit ovat edelleen kuvattu [Voit antaa käyttöoikeuden PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).


<!-- ## The PIM Security Administrator Role **PLACEHOLDER: Need description of the Security Administrator role.**-->

## <a name="roles-not-managed-in-pim"></a>Ei hallita PIM roolit

Exchange Online- tai SharePoint Online-rooleja lukuun ottamatta edellä mainittujen esitetään ei Azure AD ja siten eivät ole näkyvissä PIM. Lisätietoja näiden Office 365-palveluissa tarkasti rajattuja roolimäärityksiä muuttamisesta on artikkelissa [käyttöoikeudet Office 365: ssä](https://support.office.com/article/Permissions-in-Office-365-da585eea-f576-4f55-a1e0-87090b6aaa9d).

Azure tilaukset ja resurssiryhmät myös ei esitetään Azure AD. Azure-tilausten hallinta, katso, [miten voit lisätä tai muuttaa Azure järjestelmänvalvojan roolit](../billing-add-change-azure-subscription-administrator.md) ja saat lisätietoja Azure RBAC [Azure_Role-Based käyttöoikeuksien hallinta](role-based-access-control-configure.md).

<!--**The above links might be replaced by ones that are from within this documentation repository **-->


## <a name="user-roles-and-signing-in"></a>Liiketoiminta-roolien ja kirjautuminen
Jotkin Microsoft-palvelujen ja sovellusten määrittäminen käyttäjän roolia ei ehkä riittää, että käyttäjälle järjestelmänvalvojan.

Azure perinteinen portaalin käyttö edellyttää käyttäjä voi vain palvelun tai muiden järjestelmänvalvojat Azure-tilauksessa, vaikka käyttäjä ei tarvitse Azure-tilausten hallinta.  Esimerkiksi määritysten hallintaa perinteinen portaalissa Azure AD-käyttäjän on oltava Yleinen järjestelmänvalvoja Azure AD- ja Azure-tilauksessa tilauksen kanssasi järjestelmänvalvoja.  Lisätietoja käyttäjien lisäämisestä Azure-tilauksia, katso, [miten voit lisätä tai muuttaa Azure järjestelmänvalvojan roolit](../billing-add-change-azure-subscription-administrator.md).

Microsoft Online Services Access saattaa edellyttää käyttäjän määrittää käyttöoikeuden ennen kuin Avaa palvelun portaali tai hallintatehtäviin.

## <a name="assign-a-license-to-a-user-in-azure-ad"></a>Määrittää käyttöoikeuden käyttäjälle Azure AD-

1. Kirjautuminen [perinteinen Azure portaaliin] (http://manage.windowsazure.com) yleisen järjestelmänvalvojan tilillä tai muiden järjestelmänvalvojan tilillä.
2. Valitsee **Kaikki kohteet** päävalikosta.
3. Valitse haluamasi kansio ja, joka on liitetty käyttöoikeudet.
4. Valitse **käyttöoikeudet**. Käytettävissä olevat käyttöoikeuksien luettelo tulee näkyviin.
5. Valitse käyttöoikeus-palvelupaketti, joka sisältää käyttöoikeuksia, jonka haluat jakaa.
6. Valitse **määritettävä käyttäjille**.
7. Valitse käyttäjä, jonka haluat määrittää käyttöoikeuden.
8. Napsauta **Liitä** -painiketta.  Käyttäjä voi nyt kirjautua Azure.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Seuraavat vaiheet
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
