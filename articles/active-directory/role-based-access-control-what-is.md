<properties
    pageTitle="Roolipohjainen käyttöoikeuksien valvonta | Microsoft Azure"
    description="Aloittaminen: käyttöoikeushallinta Azure Roolipohjainen käyttöoikeuksien valvonta Azure-portaalissa. Roolimäärityksiä avulla voit määrittää hakemiston käyttöoikeuksia."
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
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="get-started-with-access-management-in-the-azure-portal"></a>Käyttöoikeuksien hallinnan Azure portaalin käytön aloittaminen

Suojauksen yrityksille kannattaa keskittyä antamalla työntekijöiden tarkka tarvittavat käyttöoikeudet. Liian monta käyttöoikeudet paljastaa hyökkääjät tilin. Liian vähän käyttöoikeudet tarkoittaa työntekijöiden ei voi käyttää tehokkaasti omia töitään. Azure Roolipohjainen käytön hallinta (RBAC) avulla voit korjata tämän ongelman Azure tarkasti rajattuja käyttöoikeushallinta tarjoamalla.

Käytä RBAC, voit eroteltava tehtäviä ryhmän ja Accessin määrä myöntäminen käyttäjille, että ne on suoritettava niiden työt. Sen sijaan, että antamalla tukeminen rajoittamaton käyttöoikeudet Azure tilaus tai resurssit, voit sallia vain tietyt toiminnot. Esimerkiksi antaa yhden työntekijän hallinnoimaan näennäiskoneiden-tilaus, kun toiseen voit hallita SQL-tietokantoja saman tilauksen piiriin kuuluvien RBAC avulla.

## <a name="basics-of-access-management-in-azure"></a>Käyttöoikeuksien hallinnan Azure-tietokannassa perusteet
Kunkin Azure tilaukseen on liitetty Azure Active Directory (AD) kansiossa. Käyttäjät, ryhmät ja kansion sovelluksia voit hallita Azure tilauksen resurssit. Määritä seuraavat Azure portal, Azure komentorivin työkalut ja Azure hallinnan API käyttöoikeudet.

Käyttöoikeuksien myöntäminen määrittämällä RBAC asianmukainen rooli käyttäjät, ryhmät ja sovellusten tietyn alueen. Roolien osoitus laajuus voi olla tilauksen, resurssiryhmä tai yksittäinen resurssi. Pääkohde-alueessa rooli antaa myös access sisältämät lasten. Esimerkiksi käyttäjä, jolla on pääsy resurssiryhmä hallita kaikki resurssit, se on, kuten sivustot ja näennäiskoneiden aliverkosta.

![Yhteyden Azure Active Directory - osien välillä kaavio](./media/role-based-access-control-what-is/rbac_aad.png)

RBAC rooli, joka määritetään määrittää käyttäjän, ryhmän tai sovelluksen voit hallita tarvittavalla alueella resursseista.

## <a name="built-in-roles"></a>Valmiit roolit
Azure RBAC on kolme perus rooleihin, jotka koskevat kaikkia resurssityypit:

- **Omistaja** on täydet oikeudet, mukaan lukien edustajakäyttöoikeudet muille resursseille.
- **Osallistuja** voi luoda ja hallita kaikki Azure resurssityyppiä, mutta ei voi myöntää käyttöoikeuksia muille.
- **Lukija** voi tarkastella nykyisiä Azure resursseja.

Azure RBAC roolien muiden Salli tietyn Azure resurssien hallintaa. Esimerkiksi virtuaalikoneen osallistujan rooli avulla käyttäjä voi luoda ja hallita näennäiskoneiden. Se ei antaa niille virtual verkon tai aliverkon virtuaalikoneen muodostaa yhteyden.

[RBAC valmiit roolit](role-based-access-built-in-roles.md) luettelo käytettävissä Azure rooleista. Määrittää toiminnot ja laajuus, joka valmiin rooleille myöntää käyttäjille. Jos haluat määrittää omia rooleja vieläkin enemmän, katso, miten voit luoda [mukautetun roolien Azure RBAC](role-based-access-control-custom-roles.md).

## <a name="resource-hierarchy-and-access-inheritance"></a>Resurssin hierarkia ja Accessin periytyminen
- Azure jokaisen **tilauksen** kuuluu vain yhden kansion.
- Kunkin **resurssiryhmä** kuuluu ainoastaan yksi tilaus.
- Kullekin **resurssille** kuuluu ainoastaan yksi resurssiryhmä.

Access, jotka voit myöntää osoitteessa ylätason käyttöalueen periytyy osoitteessa lapsen alueet. Esimerkki:

- Voit määrittää lukija Azure AD-ryhmän tilauksen-alueessa. Ryhmän jäsenet voivat tarkastella kaikissa resurssiryhmä ja resurssin tilauksen.
- Voit määrittää osallistujan rooli sovelluksen resurssien ryhmä-alueella. Se voi hallita resurssien kaikenlaisten kyseinen resurssiryhmä, mutta ei muiden resurssiryhmien tilaus.

## <a name="azure-rbac-vs-classic-subscription-administrators"></a>Azure RBAC ja perinteinen tilauksen Järjestelmänvalvojat
Perinteinen tilauksen Järjestelmänvalvojat ja apuyhteyshenkilöiden on täydet oikeudet Azure-tilaukseen. Ne voivat hallita resurssien Azure Resurssienhallinta ohjelmointirajapinnan tai [Azure perinteinen portaalia](https://manage.windowsazure.com) ja Azure perinteinen käyttöönoton mallin [Azure portaalin](https://portal.azure.com) käyttöä. Perinteinen järjestelmänvalvojat määritetään alueessa tilauksen omistaja-roolin RBAC-mallissa.

Azure portal ja uusi Azure Resurssienhallinta-ohjelmointirajapinnan tukevat Azure RBAC. Käyttäjien ja sovelluksia, jotka määritetään RBAC roolit eivät voi käyttää perinteinen hallinta-portaalin ja Azure perinteinen käyttöönottomalli.

## <a name="authorization-for-management-vs-data-operations"></a>Lupa tiedon ja hallintaa varten
Azure RBAC tukee vain hallintatoiminnot Azure resurssien Azure portaalin ja Azure Resurssienhallinta API. Se ei hyväksy kaikki tiedot tason toiminnot Azure resurssien. Esimerkiksi joku tallennustilan tilien hallinta voit antaa mutta BLOB tai tallennustilan tilin sisältämiä taulukoita voi. Vastaavasti SQL-tietokanta voidaan hallita, mutta ei sen sisältämiä taulukoita.

## <a name="next-steps"></a>Seuraavat vaiheet
- Aloittaminen [Roolipohjainen käyttöoikeuksien valvonta Azure-portaalissa](role-based-access-control-configure.md).
- Katso [RBAC valmiit roolit](role-based-access-built-in-roles.md)
- Oma [Mukautettu Azure RBAC roolien](role-based-access-control-custom-roles.md) määrittäminen
