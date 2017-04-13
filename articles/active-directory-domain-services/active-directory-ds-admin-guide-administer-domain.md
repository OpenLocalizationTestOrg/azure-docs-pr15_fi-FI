<properties
    pageTitle="Azure Active Directory-toimialueen palveluista: Hallinta hallitun toimialueen | Microsoft Azure"
    description="Azure Active Directory-toimialueen palveluista hallitun toimialueiden hallinta"
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
    ms.date="10/02/2016"
    ms.author="maheshu"/>

# <a name="administer-an-azure-active-directory-domain-services-managed-domain"></a>Hallitut Azure Active Directory-toimialueen palveluista-toimialueen hallintaan
Tässä artikkelissa kerrotaan hallinnasta hallitun Azure Active Directory (AD) toimialuepalveluita-toimialue.


## <a name="before-you-begin"></a>Ennen aloittamista
Tässä artikkelissa luetellut tehtävien suorittamiseen tarvitset:

1. Kelvollinen **Azure tilauksen**.

2. **Azure Active directory** - joko synkronoitu paikallisesta hakemistosta tai vain pilvipalveluita hakemisto.

3. **Azure AD-toimialueen palveluista** on otettava käyttöön Azure AD-kansio. Jos et ole vielä tehnyt niin, noudata kaikki [Aloitusoppaasta](./active-directory-ds-getting-started.md)kuvatut tehtävät.

4. **Toimialueen liittyneet virtuaalikoneen** , josta Azure AD-toimialueen palveluiden hallitsemiseen hallita toimialueen. Jos sinulla ei ole tällaisia virtual machine, noudata kaikki [liittyä Windows-virtual machine hallitun toimialueeseen](./active-directory-ds-admin-guide-join-windows-vm.md)liittyvä artikkelissa kuvattujen tehtävien.

5. Sinun on **"AAD Ohjauskoneen Järjestelmänvalvojat-ryhmään kuuluvat käyttäjätilin** tunnistetiedot hakemistoa hallitun toimialueen hallintaan.

<br>


## <a name="administrative-tasks-you-can-perform-on-a-managed-domain"></a>Voit suorittaa hallitun toimialueen hallintatehtävät
'AAD Ohjauskoneen Järjestelmänvalvojat-ryhmän jäsenillä on myönnetty tarvittavat oikeudet hallitun toimialueella, jotka he voivat suorittaa seuraavia tehtäviä:

- Liittää koneet hallitun toimialueeseen.

- Määritä valmiin Ryhmäkäytäntöobjekti "AADDC tietokoneet" ja 'AADDC käyttäjien' säilöjen hallitun toimialueen.

- Hallita hallitun toimialueen DNS.

- Voit luoda ja hallita mukautettuja organisaatioyksiköiden (organisaatioyksiköiden) hallitun toimialueella.

- Hallitut toimialueelle liitetty voitto tietokoneeseen järjestelmänvalvojan oikeudet.


## <a name="administrative-privileges-you-do-not-have-on-a-managed-domain"></a>Sinulla ei ole hallitun toimialueen järjestelmänvalvojan oikeudet
Toimialueen hallitsee Microsoft, mukaan lukien toiminnot, kuten korjaaminen, valvonnan ja suorittamiseen varmuuskopiot. Vuoksi toimialueen on lukittu, ja sinulla ei ole oikeuksia tiettyjä hallintatehtäviin toimialueella. Alla on joitakin esimerkkejä tehtävistä, et voi suorittaa.

- Ei ole myönnetty hallitun toimialueen toimialueen järjestelmänvalvoja tai yrityksen järjestelmänvalvojan oikeudet.

- Et voi laajentaa hallitun toimialueen rakenne.

- Et voi muodostaa yhteyttä toimialueen ohjaimet hallitun toimialueen Etätyöpöydän avulla.

- Et voi lisätä toimialueen ohjaimet hallitun toimialueeseen.


## <a name="task-1---provision-a-domain-joined-windows-server-virtual-machine-to-remotely-administer-the-managed-domain"></a>Tehtävä 1 - tarjoamista toimialueeseen liittyneet Windows Server virtual-koneen hallita hallitun toimialueen
Azure AD-toimialueen palveluista hallitun toimialueiden voi hallita tuttuja Active Directory-Valvontatyökalut esimerkiksi Active Directory järjestelmänvalvojan Center (ADAC) tai AD PowerShell. Vuokraajan järjestelmänvalvojat ei ole oikeuksia muodostaa toimialueen ohjaimet hallitun toimialueella etätyöpöydän kautta. Vuoksi 'AAD Ohjauskoneen Järjestelmänvalvojat-ryhmän jäsenet voivat hallita hallitun toimialueiden käyttäminen etäyhteyden AD Valvontatyökalut Windows Server/client tietokoneesta, johon on liitetty hallitun toimialueeseen. AD Valvontatyökalut voidaan asentaa Remote palvelimen hallinnan työkalut (RSAT) valinnainen ominaisuus, Windows Server ja hallittujen toimialueen asiakaskoneiden osana.

Ensimmäiseksi on määrittäminen Windows Server virtual laitteeseen, joka on liitetty hallitun toimialueeseen. Ohjeet Lue artikkeli [liittyä Windows Server-virtual machine hallitun Azure AD-toimialueen palvelut toimialueeseen](active-directory-ds-admin-guide-join-windows-vm.md)liittyvä.

### <a name="remotely-administer-the-managed-domain-from-a-client-computer-for-example-windows-10"></a>Hallita hallitun toimialueen asiakastietokoneesta (esimerkiksi Windows 10)
Ohjeita tämän artikkelin Windows Server-virtual machine ammattimainen AAD-Hakemistopalvelun hallita toimialueen. Kuitenkin voit myös valita Windows (esimerkiksi Windows 10) virtual asiakastietokoneessa avulla.

Voit [asentaa Remote palvelimen hallinnan työkalut (RSAT)](http://social.technet.microsoft.com/wiki/contents/articles/2202.remote-server-administration-tools-rsat-for-windows-client-and-windows-server-dsforum2wiki.aspx) Windows asiakas virtual tietokoneeseen noudattamalla TechNet-sivustossa.


## <a name="task-2---install-active-directory-administration-tools-on-the-virtual-machine"></a>Tehtävä 2 – asenna Active Directory-hallintatyökalut virtual tietokoneeseen.
Seuraavien toimien Active Directory-hallintatyökalut asennetaan toimialueen liittyneet virtuaalikoneen. [Lisätietoja asentaminen ja käyttäminen Remote palvelimen hallintatyökalujen](https://technet.microsoft.com/library/hh831501.aspx)on TechNetissä.

1. Siirry Azure perinteinen portaalissa **näennäiskoneiden** solmu. Valitse tehtävän 1 luomasi virtuaalikoneen ja valitse **Yhdistä** komentopalkista ikkunan alareunassa.

    ![Yhteyden muodostaminen Windows virtuaalikoneen](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Perinteinen portaalin pyytää sinua avaamaan tai tallentamaan tiedosto ".rdp"-tunniste, jota käytetään yhteyden muodostamiseen virtuaalikoneen. Valitse Avaa tiedosto, kun se on ladannut.

3. Kirjaudu sisään pyydettäessä käyttää 'AAD Ohjauskoneen Järjestelmänvalvojat-ryhmään kuuluvat käyttäjän tunnistetietoja. Esimerkiksi Käytämme 'bob@domainservicespreview.onmicrosoft.com' tässä tapauksessa.

4. Avaa aloitusnäytössä **Serverin hallinta**. Valitse **Lisää rooleja ja toiminnot** Serverin hallinta-ikkunassa central-ruudussa.

    ![Käynnistää virtuaalikoneen Serverin hallinta](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)

5. Valitse **Lisää rooleista ja ohjatun ominaisuuksien** **Ennen aloittamista** -sivulla **Seuraava**.

    ![Ennen aloittamista sivua](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)

6. Valitse **Asennustyyppi** -sivulla Jätä **Roolipohjainen tai ominaisuuksiin perustuvaan asennus** -vaihtoehto valittuna ja valitse **Seuraava**.

    ![Tyyppi-asennussivulla](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)

7. **Palvelimen valinta** sivulla Valitse nykyinen Virtuaalikoneen palvelimen käytetään varannon ja valitse **Seuraava**.

    ![Palvelimen valintasivu](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)

8. Valitse **Roolit** -sivulla **Seuraava**. Olemme ohittaa tämän sivun, koska emme ole asentamassa mitään rooleja palvelimessa.

9. Valitse **Ominaisuudet** -sivulla napsauttamalla Laajenna **Remote palvelimen hallintatyökalujen** -solmu ja valitse sitten Laajenna **Roolin hallintatyökalut** -solmu. Valitse rooli hallintatyökalut luettelo **AD DS ja AD LDS Työkalut** -ominaisuus.

    ![Ominaisuudet-sivulla](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-ad-tools.png)

10. Valitse **Vahvista** -sivulla asennus Mainos ja AD LDS Työkalut-toiminto virtuaalikoneen **Asenna** . Kun toiminto asennus on valmis, valitse **Sulje** Sulje ohjattu **Lisää rooleja ja ominaisuuksia** .

    ![Vahvistus-sivu](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-confirmation.png)


## <a name="task-3---connect-to-and-explore-the-managed-domain"></a>Tehtävä 3 - yhdistäminen ja hallittujen toimialueen tutkiminen
Nyt kun AD Valvontatyökalut ovat asennettuina toimialueen liittyneet virtuaalikoneen, Käytämme näitä työkaluja tutkia ja hallita hallitun toimialueen.

> [AZURE.NOTE] Sinun on oltava 'AAD Ohjauskoneen järjestelmänvalvojat'-ryhmän jäsen hallitun toimialueen hallintaan.

1. Valitse aloitusnäytössä **Valvontatyökalut**. Raportissa pitäisi näkyä virtuaalikoneen asennettuihin AD Valvontatyökalut.

    ![Palvelimeen asennetut Valvontatyökalut](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)

2. Valitse **Active Directory-hallintakeskus**.

    ![Active Directory-hallintakeskus](./media/active-directory-domain-services-admin-guide/adac-overview.png)

3. Tutustu toimialueen, napsauta toimialueen nimeä vasemmassa ruudussa (esimerkiksi contoso100.com). Huomaa, jota kutsutaan "AADDC tietokoneet" ja "AADDC käyttäjät" kaksi säilöä.

    ![ADAC - näkymän toimialueen](./media/active-directory-domain-services-admin-guide/adac-domain-view.png)

4. Napsauta kansiota, **AADDC käyttäjät** näkevät kaikki käyttäjät ja ryhmät kuuluvat hallitun toimialueen nimi. Näkyviin tulee käyttäjätilien ja Azure AD ryhmiä vuokraaja Näytä tähän säilöön ylöspäin. Tässä esimerkissä ilmoitus, kutsutaan bob"käyttäjän ja ryhmän nimeltä"AAD Ohjauskoneen järjestelmänvalvojat-käyttäjätilin ovat käytettävissä tähän säilöön.

    ![ADAC - Toimialuekäyttäjät](./media/active-directory-domain-services-admin-guide/adac-aaddc-users.png)

5. Napsauta kansiota, **AADDC tietokoneiden** näe tietokoneita hallittua tämän toimialueen nimi. Raportissa pitäisi näkyä merkinnän nykyisen virtuaalikoneen, johon on liitetty toimialueeseen. "AADDC tietokoneet"-säilö on tallennettu tietokonetilit kaikissa tietokoneissa, joissa on liitetty Azure AD-toimialueen palveluista hallitun toimialueeseen.

    ![ADAC - toimialueen liittyneet tietokoneet](./media/active-directory-domain-services-admin-guide/adac-aaddc-computers.png)

<br>

## <a name="related-content"></a>Aiheeseen liittyvää sisältöä

- [Azure AD - toimialueen palvelut-aloitusopas](./active-directory-ds-getting-started.md)

- [Liity Windows Server-virtual machine Azure AD-toimialueen palveluista hallitun toimialueeseen](active-directory-ds-admin-guide-join-windows-vm.md)

- [Ottaa käyttöön etäpalvelimeen hallintatyökalut](https://technet.microsoft.com/library/hh831501.aspx)
