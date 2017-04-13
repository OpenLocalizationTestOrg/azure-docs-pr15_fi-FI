<properties
    pageTitle="Hae aloittaminen Azure MFA pilveen | Microsoft Azure"
    description="Tämä on Microsoft Azure multi-factor authentication-sivu, jossa kuvataan Azure MFA pilveen käytön aloittaminen."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/17/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-azure-multi-factor-authentication-in-the-cloud"></a>Azure Monimenetelmäisen todentamisen pilveen käytön aloittaminen
Tässä artikkelissa käydään läpi pikaviestien käyttäminen Azure Monimenetelmäisen todentamisen pilveen.

> [AZURE.NOTE]  Seuraavat asiakirjat on tietoja siitä, miten voit antaa käyttäjien **Azure perinteinen Portal**. Jos etsit tietoja siitä, miten O365 käyttäjät Azure Monimenetelmäisen todentamisen määrittäminen, katso [Office 365: n monimenetelmäisen todentamisen määrittäminen.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)

![MFA pilveen](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## <a name="prerequisites"></a>Edellytykset
Seuraavat edellytykset ovat pakollisia, ennen kuin otat Azure Monimenetelmäisen todentamisen käyttäjiä.


1. [Azure-tilauksen Rekisteröidy](https://azure.microsoft.com/pricing/free-trial/) – Jos sinulla ei ole jo Azure tilauksen haluat kirjautumisen seuraavasti. Jos olet juuri alkuvaiheessa ja Azure MFA avulla voit käyttää kokeiluversioon
2. [Luo multi-factor Auth palveluntarjoaja](multi-factor-authentication-get-started-auth-provider.md) ja määrittää sen kansion tai [käyttäjien käyttöoikeuksien määrittäminen](multi-factor-authentication-get-started-assign-licenses.md)

> [AZURE.NOTE]  Käyttöoikeutta on käytettävissä, joilla on Azure MFA, Azure AD Premium tai yrityksen Mobility Suite (EMS).  MFA sisältyy Azure AD Premium ja EMS. Jos sinulla on tarpeeksi käyttöoikeuksia, sinun ei tarvitse luoda Auth tarjoajan.


## <a name="turn-on-two-step-verification-for-users"></a>Ota käyttöön kaksivaiheinen vahvistus on käyttäjille
Käynnistämiseen vaaditaan kaksi alkuun vahvistus käyttäjän muuttaa käyttäjän tilan käytöstä käytössä.  Lisätietoja käyttäjän tilat-kohdassa [Käyttäjän hyötyä Azure multi-factor Authentication-](multi-factor-authentication-get-started-user-states.md)

MFA käyttöön käyttäjien seuraavien ohjeiden avulla.

### <a name="to-turn-on-multi-factor-authentication"></a>Monimenetelmäisen todentamisen käyttöönotto

1.  Kirjaudu [Azure perinteinen portaaliin](https://manage.windowsazure.com) järjestelmänvalvojana.
2.  Valitse vasemmassa reunassa **Active Directorysta**.
3.  Valitse kansio, haluat ottaa käyttöön käyttäjän kansio.
![Valitse kansio](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Valitse **käyttäjät**yläreunassa.
5.  Valitse **Hallitse multi-factor Auth**sivun alareunaan. Avaa uusi selaimen välilehti.
![Valitse kansio](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Etsi käyttäjä, jonka haluat ottaa käyttöön kaksivaiheista vahvistusta varten. Joudut ehkä muuttamaan näkymän yläreunassa. Varmista, että tila on **käytöstä.** 
 ![Salli käyttäjän](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7.  Aseta **Tarkista** henkilön nimen vieressä oleva ruutu.
7.  Valitse oikealta kohdasta **Ota käyttöön**.
![Jos käyttäjä](./media/multi-factor-authentication-get-started-cloud/user1.png)
8.  Valitse **Ota käyttöön multi-factor todennus**.
![Jos käyttäjä](./media/multi-factor-authentication-get-started-cloud/enable2.png)
9.  Huomaat, että käyttäjän tilaksi muuttuu poistaminen **käytöstä** **käytössä**.
![Antaa käyttäjien](./media/multi-factor-authentication-get-started-cloud/user.png)

Kun käyttäjät on otettu käyttöön, ilmoita ne sähköpostitse. Kun seuraavan kerran, ne yritä kirjautua sisään, ne pyydetään rekisteröidä tilin kaksivaiheista vahvistusta varten. Kun he alkavat käyttää kaksivaiheista vahvistusta varten, ne on myös sovelluksen salasanat välttämiseksi lukita ulos selaimen sovellusten määrittäminen.


## <a name="use-powershell-to-automate-turning-on-two-step-verification"></a>PowerShellin avulla voit automatisoida otetaan käyttöön kaksivaiheista vahvistusta varten

Voit muuttaa [tilan](multi-factor-authentication-whats-next.md) käyttämällä [PowerShellin Azure AD](../powershell-install-configure.md)-käyttämällä seuraavasti.  Voit muuttaa `$st.State` samaksi jokin seuraavista tiloista:

- Käytössä
- Voimassa
- Ei käytössä  

> [AZURE.IMPORTANT]  Olemme avaavan vastaan käyttäjien siirtäminen suoraan käytöstä tilasta pakotettu-tilaan. Muut selainpohjaisista sovellukset lakkaavat toimimasta, koska käyttäjällä on ole läpäisseet MFA rekisteröinti ja saatujen [ohjelman salasana](multi-factor-authentication-whats-next.md#app-passwords). Jos olet selaimessa-pohjaisten sovellusten ja sovelluksen salasana on pakollinen, on suositeltavaa siirrytään ei käytössä-tilaan käytössä. Näin voit rekisteröidä ja hankkia sovelluksen salasanansa käyttäjät. Tämän jälkeen voit siirtää niitä pakotettu.

PowerShellin avulla on oltava vaihtoehdon käyttäjät joukkona. Tällä hetkellä ei Azure-portaalissa ei ole joukkona Ota käyttöön-ominaisuus, eikä sinun tarvitse valitsemalla kunkin käyttäjän erikseen. Tämä voi olla aivan tehtävän, jos sinulla on useita käyttäjiä. Luomalla PowerShell-komentosarja käyttämällä seuraavia, voit käyttäjäluettelon läpi ja otat ne.

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "\*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Tässä on esimerkki:

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "\*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }


Lisätietoja [käyttäjän hyötyä Azure multi-factor Authentication-](multi-factor-authentication-get-started-user-states.md)

## <a name="next-steps"></a>Seuraavat vaiheet
Nyt kun olet määrittänyt Azure Monimenetelmäisen todentamisen pilveen, voit määrittää ja määrittää käyttöönoton. Saat lisätietoja [Määrittäminen Azure Monimenetelmäisen todentamisen](multi-factor-authentication-whats-next.md) .
