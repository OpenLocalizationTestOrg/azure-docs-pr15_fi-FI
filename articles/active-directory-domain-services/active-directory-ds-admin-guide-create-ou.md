<properties
    pageTitle="Azure Active Directory-toimialuepalveluista: Hallinnan oppaan | Microsoft Azure"
    description="Luo organisaatioyksikkö (OU) Azure AD-toimialueen palvelut hallitun toimialueet"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="create-an-organizational-unit-ou-on-an-azure-ad-domain-services-managed-domain"></a>Luo organisaatioyksikkö (OU) hallitun Azure AD-toimialueen palvelut-toimialueella
Azure AD-toimialueen palveluista hallitun toimialueiden ovat valmiita säilöt nimeltä "AADDC tietokoneet" ja "AADDC käyttäjät". "AADDC tietokoneet" säilö on tietokoneen objekteja kaikissa tietokoneissa, joissa on liitetty hallitun toimialueeseen. 'AADDC käyttäjien' säilöä sisältää Azure AD-vuokraajan käyttäjät ja ryhmät. Joskus voi olla tarpeen luominen palvelutilejä ottamaan työmääriä hallitun toimialueella. Tähän tarkoitukseen voit luoda mukautettu organisaatioyksikkö (OU) hallitun toimialueella ja luoda palvelutilejä kyseisen OU kuluessa. Tässä artikkelissa kerrotaan OU luominen hallitun toimialueen.


## <a name="install-ad-administration-tools-on-a-domain-joined-virtual-machine-for-remote-administration"></a>Asenna AD hallintatyökalut toimialueeseen liittyneet virtual tietokoneeseen etähallinta
Azure AD-toimialueen palveluista hallitun toimialueiden voi hallita käyttämällä etäyhteyden tuttuja Active Directory-Valvontatyökalut esimerkiksi Active Directory järjestelmänvalvojan Center (ADAC) tai AD PowerShell. Vuokraajan järjestelmänvalvojat ei ole oikeuksia muodostaa toimialueen ohjaimet hallitun toimialueella etätyöpöydän kautta. Ammattimainen hallitun toimialueen asennettava AD hallinta-Työkalut-ominaisuus hallitun toimialueen virtual tietokoneeseen. Lisätietoja on artikkelissa liittyvä ohjeet [hallitun Azure AD-toimialueen palvelut-toimialueen hallintaan](active-directory-ds-admin-guide-administer-domain.md) .

## <a name="create-an-organizational-unit-on-the-managed-domain"></a>Luo organisaatioyksikkö hallitun toimialueella
Nyt kun AD Valvontatyökalut ovat asennettuina toimialueen liittyneet virtuaalikoneen, Käytämme näitä työkaluja Luo organisaatioyksikkö hallitun toimialueella. Suorita seuraavat vaiheet:

> [AZURE.NOTE] Vain "AAD Ohjauskoneen Järjestelmänvalvojat-ryhmän jäsenillä on mukautettu OU luomiseen tarvittavia oikeuksia. Varmista suorittamalla seuraavat vaiheet käyttäjänä, jolla kuuluu tähän ryhmään.

1. Valitse aloitusnäytössä **Valvontatyökalut**. Raportissa pitäisi näkyä virtuaalikoneen asennettuihin AD Valvontatyökalut.

    ![Palvelimeen asennetut Valvontatyökalut](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)

2. Valitse **Active Directory-hallintakeskus**.

    ![Active Directory-hallintakeskus](./media/active-directory-domain-services-admin-guide/adac-overview.png)

3. Voit tarkastella toimialueen, napsauta toimialueen nimeä vasemmassa ruudussa (esimerkiksi contoso100.com).

    ![ADAC - näkymän toimialueen](./media/active-directory-domain-services-admin-guide/create-ou-adac-overview.png)

4. Valitse oikealla puolella **tehtävät** -ruudun **Uusi** solmun toimialueen nimi. Tässä esimerkissä on valitsemalla **Uusi** 'contoso100(local)' solmun **tehtävät** -ruudun oikealla puolella.

    ![ADAC - uusi OU](./media/active-directory-domain-services-admin-guide/create-ou-adac-new-ou.png)

5. Raportissa pitäisi näkyä voi luoda organisaatioyksikkö. Valitse **Organisaatioyksikkö** käynnistää **Organisaatioyksikkö luominen** -valintaikkuna.

6. **Luo organisaatioyksikkö** -valintaikkunassa määrittää uuden OU **nimi** . Säätää OU lyhyt kuvaus. Voit määrittää **Hallinta** -kenttään OU varten. Jos haluat luoda mukautetun OU, valitse **OK**.

    ![ADAC – Luo OU-valintaikkuna](./media/active-directory-domain-services-admin-guide/create-ou-dialog.png)

7. Juuri luomasi OU pitäisi nyt näkyä-AD järjestelmänvalvojan Center (ADAC).

    ![ADAC – OU luotu](./media/active-directory-domain-services-admin-guide/create-ou-done.png)


## <a name="permissionssecurity-for-newly-created-ous"></a>Käyttöoikeudet ja suojauksen juuri luomasi organisaatioyksiköiden
Oletusarvon mukaan mukautettu OU luoneen käyttäjän ('AAD Ohjauskoneen Järjestelmänvalvojat-ryhmän jäsen) on myönnetty järjestelmänvalvojan oikeudet (Täydet oikeudet) OU päälle. Käyttäjä voi sitten Siirry eteenpäin ja myöntää käyttöoikeuksia muille käyttäjille tai "AAD Ohjauskoneen Järjestelmänvalvojat-ryhmään haluamallasi tavalla. Seuraavassa näyttökuvassa käyttäjän tarkastelu 'bob@domainservicespreview.onmicrosoft.com' laatijan nimi uusi 'MyCustomOU' organisaatioyksikkö myönnetään täydet päälle.

 ![ADAC - uusi OU suojaus](./media/active-directory-domain-services-admin-guide/create-ou-permissions.png)


## <a name="notes-on-administering-custom-ous"></a>Muistiinpanojen mukautetun organisaatioyksiköiden hallinnasta
Voit nyt, kun olet luonut mukautettuja OU, siirry eteenpäin ja luoda käyttäjiä, ryhmiä, tietokoneita ja palvelutilejä tämän OU. Et voi siirtää käyttäjien tai ryhmien 'AAD Ohjauskoneen käyttäjien' OU mukautetun organisaatioyksiköiden.

> [AZURE.WARNING] Käyttäjätilit, ryhmät tai palvelutilejä tietokoneen objekteja, jotka olet luonut mukautetun organisaatioyksiköiden-kohdassa eivät ole käytettävissä Azure AD-vuokraajan. Toisin sanoen objektit eivät näy Azure AD-kaavio-Ohjelmointirajapinnan käyttäminen ylös- tai Azure AD-käyttöliittymässä. Nämä objektit ovat käytettävissä vain hallitun toimialueen Azure AD-toimialueen palveluista.


## <a name="related-content"></a>Aiheeseen liittyvää sisältöä

- [Hallitut Azure AD-toimialueen palvelut-toimialueen hallintaan](active-directory-ds-admin-guide-administer-domain.md)

- [Active Directory-hallintakeskus: Käytön aloittaminen](https://technet.microsoft.com/library/dd560651.aspx)

- [Palvelutilejä vaiheittaiset ohjeet](https://technet.microsoft.com/library/dd548356.aspx)
