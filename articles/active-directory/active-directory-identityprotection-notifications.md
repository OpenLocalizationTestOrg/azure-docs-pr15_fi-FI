<properties
    pageTitle="Azure Active Directory-tunnistetietojen suojaus ilmoitukset | Microsoft Azure"
    description="Katso, miten ilmoitusten tuki tutkimuksen toimintojen."
    services="active-directory"
    keywords="Azure active Directoryn tunnistetietojen suojaus-cloud app etsiminen-sovellukset, suojaus, riski, riskin taso, heikkous, suojauskäytäntö hallinta"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory-tunnistetietojen suojaus-ilmoitukset 


Azure AD-tunnistetietojen suojauksen lähettää automaattisen ilmoituksen sähköpostitse avulla voit hallita käyttäjän riski ja risk tapahtumien kahdenlaisia:

- Käyttäjän käsiin ilmoitus sähköposti

- Viikoittainen käsittely sähköposti

## <a name="user-compromised-alert-email"></a>Käyttäjän käsiin ilmoitus sähköposti

Käyttäjän vaarantuneen sähköposti-ilmoituksen luodaan, kun Azure AD-tunnistetiedot tunnistaa tiliä, kuten käsiin. Sähköpostiviesti sisältää linkin merkitty riskin raportin tunnistetiedot Raporttinäkymät-ikkunan käyttäjät. Suosittelemme, että ilmoitukset heti tutkia käsiin.


## <a name="weekly-digest-email"></a>Viikoittainen käsittely sähköposti

Viikoittainen käsittely sähköpostin sisältää uusien riskien tapahtumien yhteenveto.<br>
Se sisältää:

- Käyttäjien vastuullasi
- Epäilyttävien toiminnot
- Havaitut heikkouksien
- Linkkejä aiheeseen liittyvät raporttien tunnistetiedot


<br>
![Korjaus](./media/active-directory-identityprotection-notifications/400.png "Remediation")
<br> 

Voit siirtyä viikoittain käsittely-sähköpostin lähettämisestä.
<br><br>
![Käyttäjän riskit](./media/active-directory-identityprotection-notifications/62.png "User risks")
<br>
 

**Aiheeseen liittyvät määritykset-valintaikkunan avaaminen**:

1. Valitse **asetukset** **Azure AD-tunnistetiedot** -sivu.
<br><br>
![Riskin käytännön](./media/active-directory-identityprotection-notifications/401.png "User risk policy")
<br>

2. Valitse **Yleiset** -osaan **ilmoitukset**.
<br><br>
![Riskin käytännön](./media/active-directory-identityprotection-notifications/405.png "User risk policy")
<br>




## <a name="see-also"></a>Katso myös

- [Azure Active Directory-tunnistetietojen suojaus](active-directory-identityprotection.md) 

