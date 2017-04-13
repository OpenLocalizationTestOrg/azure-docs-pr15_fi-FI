<properties
   pageTitle="Monimenetelmäisen todentamisen vaatiminen | Microsoft Azure"
   description="Tietoja edellyttävät monimenetelmäisen todentamisen (MFA) (sellaisten jäsenyydet) Azure Active Directory sellaisten jäsenyyksien hallinta-tunniste."
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

# <a name="how-to-require-mfa-in-azure-ad-privileged-identity-management"></a>Azure AD sellaisten jäsenyyksien hallinta MFA vaatiminen

On suositeltavaa edellyttävät monimenetelmäisen todentamisen (MFA) kaikkien oman järjestelmänvalvojat. Tämä pienentää hyökkäys vuoksi vaarantuneen salasanan riskiä.

Voit edellyttää, että käyttäjät suorittaa MFA-todennus, kun synkronointi. Blogimerkintä [MFA Office 365: ssä ja MFA Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/) vertaa, mitkä tuotteet sisältyvät Office ja Azure-tilauksia Microsoft Azure Monimenetelmäisen todentamisen tarjoaa sisältämät ominaisuudet.

Voit myös edellyttää, että käyttäjät suorittaa MFA-todennus, kun ne käyttöön Azure AD PIM roolia. Tällä tavalla, jos käyttäjä ei ole tehnyt MFA-todennus kirjautumiskerralla ne, ne pyydetään tekemään niin PIM mukaan.

## <a name="requiring-mfa-in-azure-ad-privileged-identity-management"></a>Edellyttävän MFA Azure AD-luottamuksellisten tunnistetietojen hallinta

Kun hallitset käyttäjätiedot PIM sellaisten rooli-järjestelmänvalvojana, näkyviin voi tulla ilmoituksia, jotka kannattaa MFA sellaisten tileissä. Valitse Suojausvaroitus PIM koontinäytön ja uusi sivu avautuu, jotka tarvitsevat MFA Järjestelmänvalvojatileille luettelo.  Tarvitset MFA valitsemalla useita rooleja ja valitsemalla sitten **Korjaa** -painike, tai voit yksittäisten vieressä olevaa kolmea pistettä ja valitse sitten **Korjaa** -painike.

> [AZURE.IMPORTANT] Oikea nyt Azure MFA toimii vain työpaikan tai oppilaitoksen tilit, ei Microsoft-tilit (yleensä henkilökohtainen tili, jota käytetään kirjaudutaan sisään Microsoft-palvelujen, kuten Skype, Xbox, Outlook.com yms.). Tästä syystä kuka tahansa käyttämällä Microsoft-tiliä ei voi olla olevalla järjestelmänvalvoja, koska niitä ei voi käyttää MFA aktivoimaan roolien. Jos nämä käyttäjät on edelleen hallinta työmääriä käyttämällä Microsoft-tiliä, laajentamaan ne pysyvät järjestelmänvalvojat nyt.

Voit lisäksi muuttaa tietyn roolin MFA vaatimus napsauttamalla sitä PIM Raporttinäkymät-ikkunan roolit-osassa. Valitse **asetukset** -roolin sivu ja valitsemalla sitten multi-factor authentication-kohdassa **Ota käyttöön** .

## <a name="how-azure-ad-pim-validates-mfa"></a>Miten Azure AD PIM tarkistaa MFA

On kaksi vaihtoehtoa: MFA tarkistetaan, kun käyttäjä aktivoi rooli.

Helpoin tapa on riippuvaisia Azure MFA käyttäjille, jotka ovat aktivoiminen sellaisten rooli. Toiminto, valitse ensin, että nämä käyttäjät on käyttöoikeus, jos se on tarpeen, ja olet rekisteröinyt Azure MFA. Lisätietoja tietojen hallinnasta on [Azure Monimenetelmäisen todentamisen pilveen käytön aloittaminen](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md). Se on suositeltavaa, mutta niitä ei tarvita, Määritä Azure AD voidaan valvoa MFA näiden käyttäjien, kun synkronointi. Tämä johtuu siitä MFA tarkistukset tehdään Azure AD-PIM itse.

Voit myös jos käyttäjien todentamiseen paikallisen käytössä voi olla identity-palveluntarjoajan vastuussa MFA. Jos olet määrittänyt AD liittoutumispalvelut edellyttämään älykortin lomakepohjaisen todennuksen määrittäminen ennen käyttöä Azure AD- [pilvi Securing Azure Monimenetelmäisen todentamisen ja AD FS resursseilla](../multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud.md) sisältää lähettää saatavat Azure AD määrittäminen AD FS ohjeet. Kun käyttäjä yrittää aktivoida rooli, Azure AD PIM hyväksyy, että MFA on jo vahvistettu käyttäjän kun se saa tarvittavat saatavat.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Seuraavat vaiheet
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
