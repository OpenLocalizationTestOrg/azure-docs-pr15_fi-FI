<properties
    pageTitle="Azure Active Directory-toimialueen palveluista: Hallinta hallitun toimialueiden DNS | Microsoft Azure"
    description="Azure Active Directory-toimialueen palveluista hallitun toimialueiden DNS-hallinta"
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
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="administer-dns-on-an-azure-ad-domain-services-managed-domain"></a>Hallitut Azure AD-toimialueen palvelut-toimialueen DNS hallinta
Azure Active Directory-toimialueen palveluista sisältää DNS (toimialuenimen selvitys)-palvelimeen, joka sisältää hallitun toimialueen DNS-ratkaisun. Joskus tarvitset DNS-asetusten määrittäminen hallitun toimialueella. Voit joutua koneet, joita ei ole liitetty toimialue DNS-tietueiden luominen, virtual IP-osoitteiden kuormituksen tasoitusmääritykset määrittäminen tai asennuksen ulkoiseen DNS-välittäjiä. Tästä syystä 'AAD Ohjauskoneen Järjestelmänvalvojat-ryhmään kuuluvat käyttäjät on myönnetty tarvittavat DNS-järjestelmänvalvojan oikeudet hallitun toimialueella.


## <a name="before-you-begin"></a>Ennen aloittamista
Tässä artikkelissa luetellut tehtävien suorittamiseen tarvitset:

1. Kelvollinen **Azure tilauksen**.

2. **Azure Active directory** - joko synkronoitu paikallisesta hakemistosta tai vain pilvipalveluita hakemisto.

3. **Azure AD-toimialueen palveluista** on otettava käyttöön Azure AD-kansio. Jos et ole vielä tehnyt niin, noudata kaikki [Aloitusoppaasta](./active-directory-ds-getting-started.md)kuvatut tehtävät.

4. **Toimialueen liittyneet virtuaalikoneen** , josta Azure AD-toimialueen palveluiden hallitsemiseen hallita toimialueen. Jos sinulla ei ole tällaisia virtual machine, noudata kaikki [liittyä Windows-virtual machine hallitun toimialueeseen](./active-directory-ds-admin-guide-join-windows-vm.md)liittyvä artikkelissa kuvattujen tehtävien.

5. Sinun on **"AAD Ohjauskoneen Järjestelmänvalvojat-ryhmään kuuluvat käyttäjätilin** tunnistetiedot hakemistoa hallinnoimaan hallitun toimialueesi DNS.

<br>

## <a name="task-1---provision-a-domain-joined-virtual-machine-to-remotely-administer-dns-for-the-managed-domain"></a>Tehtävä 1 - tarjoamista toimialueeseen liittyneet virtual machine hallita DNS hallitun toimialueen
Azure AD-toimialueen palveluista hallitun toimialueiden voi hallita käyttämällä etäyhteyden tuttuja Active Directory-Valvontatyökalut esimerkiksi Active Directory järjestelmänvalvojan Center (ADAC) tai AD PowerShell. Vastaavasti hallitun toimialueen DNS voidaan hallita DNS-palvelin-hallintatyökalujen etäyhteyden avulla.

Järjestelmänvalvojat Azure AD-kansiossa ei ole oikeuksia muodostaa toimialueen ohjaimet hallitun toimialueella etätyöpöydän kautta. 'AAD Ohjauskoneen Järjestelmänvalvojat-ryhmän jäsenet voivat hallita DNS hallitun toimialueiden etäyhteyden työkaluilla DNS Server Windows Server/client tietokoneesta, johon on liitetty hallitun toimialueeseen. DNS-palvelin työkalut voidaan asentaa Remote palvelimen hallinnan työkalut (RSAT) valinnainen ominaisuus, Windows Server ja hallittujen toimialueen asiakaskoneiden osana.

Ensimmäinen tehtävä on valmistelu Windows Server virtual laitteeseen, joka on liitetty hallitun toimialueeseen. Ohjeet Lue artikkeli [liittyä Windows Server-virtual machine hallitun Azure AD-toimialueen palvelut toimialueeseen](active-directory-ds-admin-guide-join-windows-vm.md)liittyvä.


## <a name="task-2---install-dns-server-tools-on-the-virtual-machine"></a>Tehtävä 2 – Asenna DNS Server tools virtuaalikoneen
Seuraavien toimien DNS-hallinta-työkalut asennetaan liitetty virtuaalikoneen. Lisätietoja [asentaminen ja käyttäminen Remote palvelimen hallintatyökalujen](https://technet.microsoft.com/library/hh831501.aspx)on TechNetissä.

1. Siirry Azure perinteinen portaalissa **näennäiskoneiden** solmu. Valitse tehtävän 1 luomasi virtuaalikoneen ja valitse **Yhdistä** komentopalkista ikkunan alareunassa.

    ![Yhteyden muodostaminen Windows virtuaalikoneen](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Perinteinen portaalin pyytää sinua avaamaan tai tallentamaan tiedosto ".rdp"-tunniste, jota käytetään yhteyden muodostamiseen virtuaalikoneen. Valitse tiedosto, kun se on ladannut.

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

9. Valitse **Ominaisuudet** -sivulla napsauttamalla Laajenna **Remote palvelimen hallintatyökalujen** -solmu ja valitse sitten Laajenna **Roolin hallintatyökalut** -solmu. Valitse **DNS Server-työkalut** -ominaisuus roolin hallintatyökalut luettelo.

    ![Ominaisuudet-sivulla](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-tools.png)

10. Valitse **Vahvista** -sivulla DNS-palvelin-Työkalut-ominaisuus asennetaan virtuaalikoneen **asentaminen** . Kun toiminto asennus on valmis, valitse **Sulje** Sulje ohjattu **Lisää rooleja ja ominaisuuksia** .

    ![Vahvistus-sivu](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-confirmation.png)


## <a name="task-3---launch-the-dns-management-console-to-administer-dns"></a>Tehtävä 3 - julkaisun DNS-hallintakonsoli hallinnoimaan DNS
Nyt kun DNS Server Tools-ominaisuus on asennettu toimialueen liittyneet virtuaalikoneen, on DNS-työkalujen avulla hallita hallitun toimialueen DNS.

> [AZURE.NOTE] Sinun on oltava ammattimainen hallitun toimialueen DNS 'AAD Ohjauskoneen järjestelmänvalvojat'-ryhmän jäsen.

1. Valitse aloitusnäytössä **Valvontatyökalut**. Raportissa pitäisi näkyä **DNS** -konsoliin virtuaalikoneen asennettuina.

    ![Järjestelmänvalvojan Työkalut – DNS-konsoliin](./media/active-directory-domain-services-admin-guide/install-rsat-dns-tools-installed.png)

2. Valitse Käynnistä DNS Management console **DNS** .

3. **DNS-palvelimeen Yhdistä** -valintaikkunassa haluamasi vaihtoehto liittyvä **Seuraava tietokone**ja hallitun toimialue (esimerkiksi contoso100.com) DNS toimialueen nimi.

    ![DNS-konsoliin - toimialueen yhdistäminen](./media/active-directory-domain-services-admin-guide/dns-console-connect-to-domain.png)

4. DNS-konsoliin muodostaa yhteyden hallitun toimialueen.

    ![DNS-konsoliin - toimialueen hallintaan](./media/active-directory-domain-services-admin-guide/dns-console-managed-domain.png)

5. Voit nyt Lisää tietokoneissa, joissa on otettu AAD toimialuepalveluita virtual verkoston DNS-tietueet DNS-konsoliin.

> [AZURE.WARNING] Ole varovainen, kun DNS-hallinta hallitun toimialueen DNS-hallinnan työkaluilla. Varmista, että olet **Älä poista tai muokata valmiita, jotka käyttävät toimialuepalveluita toimialueen DNS-tietueet**. Valmiin DNS-tietueet ovat toimialueen DNS-tietueita, nimipalvelintietueita ja muiden tietueiden käytettävän toimialueen Ohjauskoneen sijainti. Jos muokkaat näitä tietueita, toimialuepalveluita häirinneet virtual verkossa.


Katso lisätietoja hallita DNS- [DNS Työkalut artikkelissa TechNet](https://technet.microsoft.com/library/cc753579.aspx) .


## <a name="related-content"></a>Aiheeseen liittyvää sisältöä

- [Azure AD - toimialueen palvelut-aloitusopas](./active-directory-ds-getting-started.md)

- [Liity Windows Server-virtual machine Azure AD-toimialueen palveluista hallitun toimialueeseen](active-directory-ds-admin-guide-join-windows-vm.md)

- [Hallitut Azure AD-toimialueen palvelut-toimialueen hallintaan](active-directory-ds-admin-guide-administer-domain.md)

- [DNS-hallintatyökalut](https://technet.microsoft.com/library/cc753579.aspx)
