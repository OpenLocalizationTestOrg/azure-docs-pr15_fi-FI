<properties
   pageTitle="Ohjatun suojaustoiminnon Azure AD sellaisten jäsenyyksien hallinta"
   description="Ensimmäisen kerran, voit käyttää Azure Active Directory sellaisten jäsenyyksien hallinta-tunniste, näyttöön tulee suojauksen ohjatun toiminnon avulla. Tässä artikkelissa kuvataan, miten ohjatun luomisen."
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

# <a name="the-azure-ad-privileged-identity-management-security-wizard"></a>Ohjatun suojaustoiminnon Azure AD sellaisten jäsenyyksien hallinta

Jos ole kohtaan ensimmäinen henkilö suorittaa Azure sellaisten käyttäjätietojen hallinta (PIM) organisaatiollesi, voit valita jommankumman ohjatun toiminnon avulla. Ohjattu toiminto auttaa ymmärtämään suojausriskejä sellaisten käyttäjätietoja ja niiden alentaminen PIM riskejä. Et tarvitse tehdä muutoksia aiemmin roolimäärityksiä ohjatussa, jos haluat myöhemmin.

## <a name="what-to-expect"></a>Toiminta

Ennen kuin organisaation aloittaa käyttämällä PIM, kaikki roolimäärityksiä ovat pysyviä: käyttäjät ovat aina rooli vaikka he eivät ole tarvitse niiden oikeudet.  Ohjatun toiminnon ensimmäisessä vaiheessa näkyy suuri toimittajien rooleja ja kuinka monta käyttäjää on tällä hetkellä rooleja. Voi siirtyä tietyn roolin Saat lisätietoja käyttäjien, jos sellainen tai lisää ne ovat tuntemattomia.

Ohjatun toiminnon toinen vaihe ansiosta voit muuttaa järjestelmänvalvojan roolimäärityksiä.  

> [AZURE.WARNING]On tärkeää, että sinulla on vähintään yksi yleinen järjestelmänvalvoja ja useamman kuin yhden sellaisten roolin järjestelmänvalvojan organisaatiotilillä (ei ole Microsoft-tili). Jos näkyvissä on vain yksi sellaisten rooli-järjestelmänvalvoja, organisaation ei voi hallita PIM, jos tilin poistetaan.
> Myös pitää roolimäärityksiä pysyvästi, jos käyttäjällä on Microsoft-tiliä (tili, jota hän joko Kirjaudu sisään Microsoft-palvelujen, kuten Skype ja Outlook.com). Jos aiot vaadi MFA roolin aktivointi, käyttäjän on lukittuna.


Kun olet tehnyt muutokset, ohjattu toiminto ei enää näytetä. Kun seuraavan kerran, sinun tai toisen sellaisten roolin järjestelmänvalvojan käyttää PIM, näet PIM Raporttinäkymät-ikkunan.  

- Jos haluat voit lisätä tai poistaa käyttäjien rooleja tai muuttaa varaukset pysyviä, olevalla, Lue lisää, [kuinka voit lisätä tai poistaa käyttäjän rooli](active-directory-privileged-identity-management-how-to-add-role-to-user.md).
- Jos haluat antaa lisätietoja käyttäjille hallittavan PIM, Lisätietoja on artikkelissa miten [Voit antaa käyttöoikeuden PIM hallinta](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).



## <a name="next-steps"></a>Seuraavat vaiheet
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
